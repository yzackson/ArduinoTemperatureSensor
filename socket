/*
 * teste.c
 *
 *  Created on: 4 de jun de 2018
 *      Author: Isaac
 */


#include<stdio.h>
#include<stdlib.h>
#include<windows.h>
#include<winsock2.h>
#include<winsock.h>
#include<conio.h> //kbhit(); -> pega a ultima tecla do teclado
#include<string.h>


WSADATA wsa;
SOCKET s;
struct sockaddr_in server;
char *message , server_reply[2000], ipAux[15];
int recv_size;

//---------------------------------------------------------------------------------------------------

void IniciaWinsock ()
{
    printf("\nInitialising Winsock...");
    if (WSAStartup(MAKEWORD(2,2),&wsa) != 0)
    {
        printf("Failed. Error Code : %d",WSAGetLastError());
        //return 1;
    }
    printf("Initialised.\n");
    //return 0;
}

//---------------------------------------------------------------------------------------------------

void CriaWinsock ()
{
    if((s = socket(AF_INET , SOCK_STREAM , 0 )) == INVALID_SOCKET)
    {
        printf("Could not create socket : %d" , WSAGetLastError());
    }

    printf("Socket created.\n");
}

//---------------------------------------------------------------------------------------------------

/*

void Available()
{
	server.sin_addr.s_addr = inet_addr(ipAux);

	if (connect(s , (struct sockaddr *)&server , sizeof(server)) >= 0)
	{
		printf (ipAux);
	    puts(" Disponнvel");
	}else{
		printf (ipAux);
		puts(" Indisponнvel");
	}
}

*/

//---------------------------------------------------------------------------------------------------

void Select()
{
	puts("What IP do you want connect?");
	scanf("%s", ipAux);
	fflush(stdin);
}

//---------------------------------------------------------------------------------------------------

float ConnectIP()
{
	server.sin_addr.s_addr = inet_addr(ipAux);
	return (connect(s , (struct sockaddr *)&server , sizeof(server)));
}

//---------------------------------------------------------------------------------------------------

char Command ()
{
	char x;
    //Enviar comando ao arduнno
	puts("\n");
   	puts("'i' for information");
   	puts("'r' for take the data (sensor)");
   	puts("'w' for act (actuador)");
   	puts("'s' for exit");
   	scanf("%c",&x);
   	fflush(stdin);
   	return x;
}

//---------------------------------------------------------------------------------------------------

void Command_I ()
{
	message = "i";
	send(s , message , strlen(message) , 0);
	recv(s , server_reply , 2000 , 0);
	printf("%s\n",server_reply);
}

//---------------------------------------------------------------------------------------------------

void Command_R()
{
	message = "r";
	send(s , message , strlen(message) , 0);
	recv(s , server_reply , 2000 , 0);
	//printf("%s\n",server_reply);
	printf("%s\n",server_reply);
}

//---------------------------------------------------------------------------------------------------

void Command_W()
{
	int i;
	char x[4];
	printf ("Enter with the value for act: ");
	scanf ("%i",&i);
	sprintf(ipAux,"w%d",i);
	send(s , x , strlen(message) , 0);
	recv(s , server_reply , 2000 , 0);

	if((recv_size = recv(s , server_reply , 2000 , 0)) == SOCKET_ERROR)
	{
	    puts("The act could not operate\n");
	}
	puts("Sucess!\n");

}

///////////////////-ESCOPO PRINCIPAL-/////////////////////////////
int main(int argc , char *argv[])
{
	setvbuf(stdout, NULL, _IONBF, 0);
	setvbuf(stderr, NULL, _IONBF, 0);

	server.sin_family = AF_INET;
	server.sin_port = htons( 80 );

	char aux;


//-------------INICIA WINSOCK-------------//
	IniciaWinsock ();


//-------------CRIA WINSOCK--------------//
	CriaWinsock();


//----------DESCOBERTA DE REDE----------//

/*

	puts("See network... Please wait...");
	puts("IPs Avaibles:\n");

	for (int i=98;i<=99;i++)
	{
		sprintf(ipAux,"192.168.1.%d",i);
		Available();
	}

*/

//------------SELECIONAR IP-------------//
	Select();


//--------------CONECTAR----------------//
	float computer;
	computer = ConnectIP();
	if (computer < 0)
	{
		puts("error");
		Select();
		computer = ConnectIP();
	}else{
		puts("Connected!");
	}
//--------------Comandos--------------//

	aux = Command();
   	while ((aux!='s') || (aux!='S'))
    {
   		if ((aux == 'i') || (aux=='I'))
       	{
   			Command_I();
   		}else{
   			if (aux == 'r')
   			{
   				puts ("After start, when want, press any key for exit");
   			   	system("pause");
   			   	while (!kbhit())
   			   	{
   			   		Command_R();
   			   		fflush(stdin);
   			   	}
   			   	system("pause");
   			}else{
   				if (aux == 'w')
   				{
   					Command_W();
   				}
   			}
   		}
   		aux = Command();
    }

   	system("pause");
    return 0;
}
