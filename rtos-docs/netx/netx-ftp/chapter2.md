---
title: Kapitel 2 – Installation och användning av Azure RTOS NetX FTP
description: Det här kapitlet innehåller en beskrivning av olika problem som rör installation, installation och användning av Azure RTOS NetX FTP-komponenten.
author: philmea
ms.author: philmea
ms.date: 06/04/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 9da2761ed9483a920ab6f735b8a3a6bd82936c867ece8047b622788d5fb99804
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/07/2021
ms.locfileid: "116799497"
---
# <a name="chapter-2---installation-and-use-of-azure-rtos-netx-ftp"></a>Kapitel 2 – Installation och användning av Azure RTOS NetX FTP

Det här kapitlet innehåller en beskrivning av olika problem som rör installation, installation och användning av Azure RTOS NetX FTP-komponenten.

## <a name="product-distribution"></a>Produktdistribution

Azure RTOS NetX kan hämtas från vår offentliga källkodsdatabas på [https://github.com/azure-rtos/netx/](https://github.com/azure-rtos/netx/]) . Paketet innehåller två källfiler och en PDF-fil som innehåller det här dokumentet, enligt följande:

- **nx_ftp.h:** Rubrikfil för FTP för NetX
- **nx_ftp_client.c:** C-källfil för FTP-klient för NetX
- **nx_ftp_server.c:** C-källfil för FTP-server för NetX
- **filex_stub.h:** Stub-fil om FileX inte finns
- **nx_ftp.pdf:** PDF-beskrivning av FTP för NetX
- **demo_netx_ftp.c:** FTP-demonstrationssystem

## <a name="ftp-installation"></a>FTP-installation

För att kunna använda FTP för NetX ska hela distributionen som nämns ovan kopieras till samma katalog där NetX är installerat. Om NetX till exempel är installerat i katalogen "*\threadx\arm7\green*" ska *filerna nx_ftp.h* *, nx_ftp_client.c* och *nx_ftp_server.c* kopieras till den här katalogen.

## <a name="using-ftp"></a>Använd FTP

Det är enkelt att använda FTP för NetX. I princip måste programkoden innehålla *nx_ftp.h* efter att den innehåller *tx_api.h, fx_api.h* och *nx_api.h* för att kunna använda ThreadX, FileX respektive NetX. När *nx_ftp.h* ingår kan programkoden sedan göra FTP-funktionsanrop som anges senare i den här guiden. Programmet måste även innehålla *nx_ftp_client.c* *och nx_ftp_server.c* i byggprocessen. Dessa filer måste kompileras på samma sätt som andra programfiler och dess objektformulär måste länkas tillsammans med programmets filer. Det här är allt som krävs för att använda NetX FTP.

> [!NOTE]
> Eftersom FTP använder NetX TCP-tjänster måste TCP aktiveras med nx_tcp_enable *innan* DU använder FTP.

## <a name="small-example-system"></a>Litet exempelsystem

Ett exempel på hur enkelt det är att använda NetX FTP beskrivs i bild 1.1 som visas nedan.

> [!NOTE]
> Det här är för en värdenhet med ett enda nätverksgränssnitt.

I det här exemplet kommer *FTP-indelningsfilen nx_ftp_client.h* *och nx_ftp_server.h* på rad 10 och 11. Därefter skapas FTP-servern i "*tx_application_define*" på rad 134. Observera att FTP-serverkontrollblocket "*Server*" definierades som en global variabel på rad 31 tidigare. När det har skapats startas en FTP-server på rad 363. På rad 183 skapas FTP-klienten. Slutligen skriver klienten filen på rad 229 och läser tillbaka filen på rad 318.

```c
/* This is a small demo of NetX FTP on the high-performance NetX TCP/IP stack. This demo
relies on ThreadX, NetX, and FileX to show a simple file transfer from the client
and then back to the server. */

#include     "tx_api.h"
#include     "fx_api.h"
#include     "nx_api.h"
#include     "nx_ftp_client.h"
#include     "nx_ftp_server.h"

#define     DEMO_STACK_SIZE     4096

/* Define the ThreadX, NetX, and FileX object control blocks... */

TX_THREAD          server_thread;
TX_THREAD          client_thread;
NX_PACKET_POOL     server_pool;
NX_IP              server_ip;
NX_PACKET_POOL     client_pool;
NX_IP              client_ip;
FX_MEDIA           ram_disk;

/* Define the NetX FTP object control blocks. */

NX_FTP_CLIENT     ftp_client;
NX_FTP_SERVER     ftp_server;

/* Define the counters used in the demo application... */

ULONG     error_counter = 0;

/* Define the memory area for the FileX RAM disk. */

UCHAR     ram_disk_memory[32000];
UCHAR     ram_disk_sector_cache[512];

#define FTP_SERVER_ADDRESS IP_ADDRESS(1,2,3,4)
#define FTP_CLIENT_ADDRESS IP_ADDRESS(1,2,3,5)

extern UINT _fx_media_format(FX_MEDIA *media_ptr, VOID (*driver)(FX_MEDIA *media),
                    VOID *driver_info_ptr, UCHAR *memory_ptr, UINT memory_size,
                    CHAR *volume_name, UINT number_of_fats, UINT directory_entries,
                    UINT hidden_sectors, ULONG total_sectors, UINT bytes_per_sector,
                    UINT sectors_per_cluster, UINT heads, UINT sectors_per_track);

/* Define the FileX and NetX driver entry functions. */
VOID     _fx_ram_driver(FX_MEDIA *media_ptr);

/* Replace the 'ram' driver with your own Ethernet driver. */
VOID     _nx_ram_network_driver(NX_IP_DRIVER *driver_req_ptr);

void     client_thread_entry(ULONG thread_input);
void     thread_server_entry(ULONG thread_input);

/* Define server login/logout functions. These are stubs for functions that would
validate a client login request. */

UINT     server_login(struct NX_FTP_SERVER_STRUCT *ftp_server_ptr,
                ULONG client_ip_address, UINT client_port, CHAR *name,
                CHAR *password, CHAR *extra_info);

UINT     server_logout(struct NX_FTP_SERVER_STRUCT *ftp_server_ptr,
                    ULONG client_ip_address, UINT client_port, CHAR *name,
                    CHAR *password, CHAR *extra_info);

/* Define main entry point. */

int main()
{

    /* Enter the ThreadX kernel. */
    tx_kernel_enter();
    return(0);
}

/* Define what the initial system looks like. */

void     tx_application_define(void *first_unused_memory)
{

UINT      status;
UCHAR     *pointer;

    /* Setup the working pointer. */
    pointer = (UCHAR *) first_unused_memory;

    /* Create a helper thread for the server. */
    tx_thread_create(&server_thread, "FTP Server thread", thread_server_entry,
                    0, pointer, DEMO_STACK_SIZE,
                    4, 4, TX_NO_TIME_SLICE, TX_AUTO_START);

    pointer = pointer + DEMO_STACK_SIZE;

    /* Initialize NetX. */
    nx_system_initialize();

    /* Create the packet pool for the FTP Server. */
    status = nx_packet_pool_create(&server_pool, "NetX Server Packet Pool",
                                    256, pointer, 8192);
    pointer = pointer + 8192;

    /* Check for errors. */
    if (status)
        error_counter++;

    /* Create the IP instance for the FTP Server. */
    status = nx_ip_create(&server_ip, "NetX Server IP Instance",
                        FTP_SERVER_ADDRESS, 0xFFFFFF00UL, &server_pool,
                        _nx_ram_network_driver, pointer, 2048, 1);
    pointer = pointer + 2048;

    /* Check status. */
    if (status != NX_SUCCESS)
    {
        error_counter++;
        return;
    }

    /* Enable ARP and supply ARP cache memory for server IP instance. */
    nx_arp_enable(&server_ip, (void *) pointer, 1024);
    pointer = pointer + 1024;

    /* Enable TCP. */
    nx_tcp_enable(&server_ip);

    /* Create the FTP server. */
    status = nx_ftp_server_create(&ftp_server, "FTP Server Instance",
                    &server_ip, &ram_disk, pointer, DEMO_STACK_SIZE,
                    &server_pool, server_login, server_logout);
    pointer = pointer + DEMO_STACK_SIZE;

    /* Check status. */
    if (status != NX_SUCCESS)
    {
        error_counter++;
        return;
    }

    /* Now set up the FTP Client. */

    /* Create the main FTP client thread. */
    status = tx_thread_create(&client_thread, "FTP Client thread ",
                            client_thread_entry, 0,
                            pointer, DEMO_STACK_SIZE,
                            6, 6, TX_NO_TIME_SLICE, TX_AUTO_START);
    pointer = pointer + DEMO_STACK_SIZE;

    /* Check status. */
    if (status != NX_SUCCESS)
    {
        error_counter++;
        return;
    }

    /* Create a packet pool for the FTP client. */
    status = nx_packet_pool_create(&client_pool, "NetX Client Packet Pool",
                                    256, pointer, 8192);
    pointer = pointer + 8192;

    /* Create an IP instance for the FTP client. */
    status = nx_ip_create(&client_ip, "NetX Client IP Instance", FTP_CLIENT_ADDRESS,
            0xFFFFFF00UL, &client_pool, _nx_ram_network_driver, pointer, 2048, 1);
    pointer = pointer + 2048;

    /* Enable ARP and supply ARP cache memory for the FTP Client IP. */
    nx_arp_enable(&client_ip, (void *) pointer, 1024);

    pointer = pointer + 1024;

    /* Enable TCP for client IP instance. */
    nx_tcp_enable(&client_ip);

    return;
}

/* Define the FTP client thread. */
void     client_thread_entry(ULONG thread_input)
{

NX_PACKET     *my_packet;
UINT          status;

    /* Format the RAM disk - the memory for the RAM disk was defined above. */
    status = _fx_media_format(&ram_disk,
            _fx_ram_driver, /* Driver entry */
            ram_disk_memory, /* RAM disk memory pointer */
            ram_disk_sector_cache, /* Media buffer pointer */
            sizeof(ram_disk_sector_cache), /* Media buffer size */
            "MY_RAM_DISK", /* Volume Name */
            1, /* Number of FATs */
            32, /* Directory Entries */
            0, /* Hidden sectors */
            256, /* Total sectors */
            128, /* Sector size */
            1, /* Sectors per cluster */
            1, /* Heads */
            1); /* Sectors per track */

    /* Check status. */
    if (status != NX_SUCCESS)
    {
        error_counter++;
        return;
    }

    /* Open the RAM disk. */
    status = fx_media_open(&ram_disk, "RAM DISK", _fx_ram_driver, ram_disk_memory,
                            ram_disk_sector_cache, sizeof(ram_disk_sector_cache));

    /* Check status. */
    if (status != NX_SUCCESS)
    {
        error_counter++;
        return;
    }

     /* Let the IP threads and driver initialize the system. */
    tx_thread_sleep(100);

    /* Create an FTP client. */
    status = nx_ftp_client_create(&ftp_client, "FTP Client", &client_ip, 2000, &client_pool);

    /* Check status. */
    if (status != NX_SUCCESS)
    {

        error_counter++;
        return;
    }

    printf("Created the FTP Client\n");

    /* Now connect with the NetX FTP (IPv4) server. */
    status = nx_ftp_client_connect(&ftp_client, FTP_SERVER_ADDRESS,
                                    "name", "password", 100);

    /* Check status. */
    if (status != NX_SUCCESS)
    {

        error_counter++;
        return;
    }

    printf("Connected to the FTP Server\n");

    /* Open a FTP file for writing. */
    status = nx_ftp_client_file_open(&ftp_client, "test.txt", NX_FTP_OPEN_FOR_WRITE, 100);

    /* Check status. */
    if (status != NX_SUCCESS)
    {

        error_counter++;
        return;
    }

    printf("Opened the FTP client test.txt file\n");

    /* Allocate a FTP packet. */
    status = nx_packet_allocate(&client_pool, &my_packet, NX_TCP_PACKET, 100);

    /* Check status. */
    if (status != NX_SUCCESS)
    {

        error_counter++;
        return;
    }

    /* Write ABCs into the packet payload! */
    memcpy(my_packet -> nx_packet_prepend_ptr, "ABCDEFGHIJKLMNOPQRSTUVWXYZ ", 28);

    /* Adjust the write pointer. */
    my_packet -> nx_packet_length = 28;
    my_packet -> nx_packet_append_ptr = my_packet -> nx_packet_prepend_ptr + 28;

    /* Write the packet to the file test.txt. */
    status = nx_ftp_client_file_write(&ftp_client, my_packet, 100);

    /* Check status. */
    if (status != NX_SUCCESS)
    {
        error_counter++;
    }
    else
        printf("Wrote to the FTP client test.txt file\n");

    /* Close the file. */
    status = nx_ftp_client_file_close(&ftp_client, 100);

    /* Check status. */
    if (status != NX_SUCCESS)
        error_counter++;
    else
        printf("Closed the FTP client test.txt file\n");

    /* Now open the same file for reading. */
    status = nx_ftp_client_file_open(&ftp_client, "test.txt", NX_FTP_OPEN_FOR_READ, 100);

    /* Check status. */
    if (status != NX_SUCCESS)
        error_counter++;

    else
        printf("Reopened the FTP client test.txt file\n");

    /* Read the file. */
    status = nx_ftp_client_file_read(&ftp_client, &my_packet, 100);

    /* Check status. */
    if (status != NX_SUCCESS)
        error_counter++;
    else
    {
        printf("Reread the FTP client test.txt file\n");
        nx_packet_release(my_packet);
    }

    /* Close this file. */
    status = nx_ftp_client_file_close(&ftp_client, 100);

    if (status != NX_SUCCESS)
        error_counter++;

    /* Disconnect from the server. */
    status = nx_ftp_client_disconnect(&ftp_client, 100);

    /* Check status. */
    if (status != NX_SUCCESS)
        error_counter++;

    /* Delete the FTP client. */
    status = nx_ftp_client_delete(&ftp_client);

    /* Check status. */
    if (status != NX_SUCCESS)
        error_counter++;
}

/* Define the helper FTP server thread. */
void     thread_server_entry(ULONG thread_input)
{

UINT     status;

    /* Wait till the IP thread and driver have initialized the system. */
    tx_thread_sleep(100);

    /* OK to start the FTP Server. */
    status = nx_ftp_server_start(&ftp_server);

    if (status != NX_SUCCESS)
        error_counter++;

    printf("Server started!\n");

    /* FTP server ready to take requests! */

    /* Let the IP threads execute. */
    tx_thread_relinquish();

    return;
}

UINT     server_login(struct NX_FTP_SERVER_STRUCT *ftp_server_ptr,
                ULONG client_ip_address, UINT client_port,
                CHAR *name, CHAR *password, CHAR *extra_info)
{

    printf("Logged in!\n");
    /* Always return success. */
    return(NX_SUCCESS);
}

UINT     server_logout(struct NX_FTP_SERVER_STRUCT *ftp_server_ptr,
                    ULONG client_ip_address, UINT client_port,
                    CHAR *name, CHAR *password, CHAR *extra_info)
{
    printf("Logged out!\n");

    /* Always return success. */
    return(NX_SUCCESS);
}
```

Bild 1.1 Exempel på FTP-klient och server med NetX (värd för ett enda nätverksgränssnitt)

## <a name="configuration-options"></a>Konfigurationsalternativ

Det finns flera konfigurationsalternativ för att skapa FTP för NetX. I följande lista beskrivs var och en i detalj:  

- **NX_FTP_SERVER_PRIORITY:** FTP-servertrådens prioritet. Som standard definieras det här värdet som 16 för att ange prioritet 16.

- **NX_FTP_MAX_CLIENTS:** Det maximala antalet klienter som servern kan hantera samtidigt. Som standard är det här värdet 4 för att stödja 4 klienter samtidigt.

- **NX_FTP_SERVER_MIN_PACKET_PAYLOAD:** Den minsta storleken på nyttolasten för serverpaketpoolen i byte, inklusive TCP, IP och nätverksramrubriker plus HTTP-data. Standardvärdet är 256 (maximal längd på filnamn i FileX) + 12 byte för filinformation och NX_PHYSICAL_TRAILER.

- **NX_FTP_NO_FILEX:** Det här alternativet tillhandahåller en stub för FileX-beroenden. FTP-klienten fungerar utan ändringar om det här alternativet har definierats. FTP-servern måste antingen ändras eller så måste användaren skapa ett fåtal FileX-tjänster för att fungera korrekt.

- **NX_FTP_CONTROL_TOS: Typ** av tjänst som krävs för FTP TCP-kontrollbegäranden. Som standard definieras det här värdet som NX_IP_NORMAL för att indikera normal IP-pakettjänst. Den här definierar kan anges av programmet innan du tar med *nx_ftp.h*.

- **NX_FTP_DATA_TOS:** Typ av tjänst som krävs för FTP TCP-databegäranden. Som standard definieras det här värdet som NX_IP_NORMAL för att indikera normal IP-pakettjänst. Den här definierar kan anges av programmet innan du tar med *nx_ftp.h*.

- **NX_FTP_FRAGMENT_OPTION:** Aktivera fragment för FTP TCP-begäranden. Som standard är det här värdet NX_DONT_FRAGMENT inaktivera FTP TCP-fragmentering. Den här definierar kan anges av programmet innan du tar med *nx_ftp.h*.

- **NX_FTP_CONTROL_WINDOW_SIZE:** Kontrollera socketfönstrets storlek. Som standard är det här värdet 400 byte. Den här definierar kan anges av programmet innan du tar med *nx_ftp.h*.

- **NX_FTP_DATA_WINDOW_SIZE:** Fönsterstorlek för datasocket. Som standard är det här värdet 2 048 byte. Den här definierar kan anges av programmet innan du tar med *nx_ftp.h*.

- **NX_FTP_TIME_TO_LIVE:** Anger antalet routrar som det här paketet kan passera innan det tas bort. Standardvärdet är inställt på 0x80, men kan definieras om innan du tar *nx_ftp.h.*

- **NX_FTP_SERVER_TIMEOUT:** Anger antalet ThreadX-tick som interna tjänster ska pausa för. Standardvärdet är inställt på 100, men kan definieras om innan du tar *med nx_ftp.h.*

- **NX_FTP_USERNAME_SIZE:** Anger antalet byte som tillåts i ett användarnamn som angetts av *klienten.* Standardvärdet är inställt på 20, men kan definieras om innan du tar *med nx_ftp.h.*

- **NX_FTP_PASSWORD_SIZE:** Anger antalet byte som tillåts i ett lösenord som anges av *klienten.* Standardvärdet är inställt på 20, men kan definieras om innan du tar *med nx_ftp.h.*

- **NX_FTP_ACTIVITY_TIMEOUT:** Anger antalet sekunder som en klientanslutning upprätthålls om det inte finns någon aktivitet. Standardvärdet är inställt på 240, men kan definieras om innan du *tar med nx_ftp.h.*

- **NX_FTP_TIMEOUT_PERIOD:** Anger antalet sekunder mellan serverkontrollen för klient inaktivitet. Standardvärdet är inställt på 60, men kan definieras om innan du tar *nx_ftp.h.*
