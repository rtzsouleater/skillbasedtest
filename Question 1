#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <unistd.h>

#define BUFFER_SIZE 1024

void start_client() {
    char server_ip[INET_ADDRSTRLEN];
    int server_port;
    char user_input[BUFFER_SIZE];
    int client_socket;
    struct sockaddr_in server_address;

    printf("Enter server IP address: ");
    fgets(server_ip, INET_ADDRSTRLEN, stdin);
    server_ip[strcspn(server_ip, "\n")] = '\0';

    printf("Enter server port number: ");
    scanf("%d", &server_port);
    getchar(); // Consume the newline character

    printf("Enter a string: ");
    fgets(user_input, BUFFER_SIZE, stdin);
    user_input[strcspn(user_input, "\n")] = '\0';

    client_socket = socket(AF_INET, SOCK_STREAM, 0);
    if (client_socket == -1) {
        perror("Socket creation failed");
        exit(1);
    }

    server_address.sin_family = AF_INET;
    server_address.sin_port = htons(server_port);

    if (inet_pton(AF_INET, server_ip, &(server_address.sin_addr)) <= 0) {
        perror("Invalid address or address not supported");
        exit(1);
    }

    if (connect(client_socket, (struct sockaddr*)&server_address, sizeof(server_address)) < 0) {
        perror("Connection failed");
        exit(1);
    }

    send(client_socket, user_input, strlen(user_input), 0);

    char buffer[BUFFER_SIZE];
    ssize_t bytes_received = recv(client_socket, buffer, BUFFER_SIZE, 0);
    if (bytes_received > 0) {
        buffer[bytes_received] = '\0';
        printf("Server reply: %s\n", buffer);
    }

    close(client_socket);
}

int main() {
    start_client();
    return 0;
}
