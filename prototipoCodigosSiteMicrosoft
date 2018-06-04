/*
 * create_a_basic_winsock_application.c
 *
 *  Created on: 4 de jun de 2018
 *      Author: Isaac Aragão
 */

// Este define é para não gerar conflito entre o header winsock2.h e windows.h, pois o winsock2.h tem alguns elementos do windows.h, o que pode gerar algum conflito em algum momento
#ifndef WIN32_LEAN_AND_MEAN
#define WIN32_LEAN_AND_MEAN // The WIN32_LEAN_AND_MEAN macro prevents the Winsock.h from being included by the Windows.h header.
#endif


#include <windows.h>
#include <winsock2.h> // Biblioteca básica para criação de Sockets
#include <ws2tcpip.h> // Adiciona funções sobre TCP/IP ao programa
#include <iphlpapi.h> // O header Iphlpapi.h é necessário se uma aplicação estiver usando o IP Helper APIs e, se for utilizada tem que ser incluída depois do winsock2.h
#include <stdio.h> // Standard Library

#pragma comment(lib, "Ws2_32.lib") //Applications that use Winsock must be linked with the Ws2_32.lib library file. The #pragma comment indicates to the linker that the Ws2_32.lib file is needed


int main() {

//************************** To start the WinSock ***************************************
	int iResult;

		// Initialize Winsock
		iResult = WSAStartup(MAKEWORD(2,2), &wsaData); //The MAKEWORD(2,2) parameter of WSAStartup makes a request for version 2.2 of Winsock on the system
		if (iResult != 0) {
		    printf("WSAStartup failed: %d\n", iResult);
		    return 1;
		}
//***************************************************************************************
		
		struct addrinfo *result = NULL,
							*ptr = NULL,
							hints;
			
			ZeroMemory( &hints, sizeof(hints) );
			hints.ai_family = AF_INET; // Define what kind of IP the client will use to connect (default = AF_UNSPEC "May be anyone of both" - If the client application wants to connect using only IPv6 or IPv4, then the address family needs to be set to AF_INET6 for IPv6 or AF_INET for IPv4 in the hints parameter.
			hints.ai_socktype = SOCK_STREAM;
			hints.ai_protocol = IPPROTO_TCP;
			
			
			#define DEFAULT_PORT "27015"
			
			// Resolve the server address and port
			iResult = getaddrinfo(argv[1], DEFAULT_PORT, &hints, &result);
			if (iResult != 0) {
				printf("getaddrinfo failed: %d\n", iResult);
				WSACleanup();
				return 1;
			}
			
			
			SOCKET ConnectSocket = INVALID_SOCKET;
			
			
			// Attempt to connect to the first address returned by
			// the call to getaddrinfo
			ptr=result;

			// Create a SOCKET for connecting to server
			ConnectSocket = socket(ptr->ai_family, ptr->ai_socktype, 
			    ptr->ai_protocol);
			
			
			// Check if the connected socket is a valid socket
			if (ConnectSocket == INVALID_SOCKET) {
			    printf("Error at socket(): %ld\n", WSAGetLastError());
			    freeaddrinfo(result);
			    WSACleanup();
			    return 1;
			}
			
			
			
			
			// Connect to server.
			iResult = connect( ConnectSocket, ptr->ai_addr, (int)ptr->ai_addrlen);
			if (iResult == SOCKET_ERROR) {
			    closesocket(ConnectSocket);
			    ConnectSocket = INVALID_SOCKET;
			}

			// Should really try the next address returned by getaddrinfo
			// if the connect call failed
			// But for this simple example we just free the resources
			// returned by getaddrinfo and print an error message

			freeaddrinfo(result);

			if (ConnectSocket == INVALID_SOCKET) {
			    printf("Unable to connect to server!\n");
			    WSACleanup();
			    return 1;
			}

	return 0;
}
