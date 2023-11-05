#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <windows.h>


#define MAX_DESTINATIONS 10
#define MAX_BUSES 50
#define MAX_TICKETS 100
#define USERNAME_EMPLOYEE "barasa"
#define PASSWORD_EMPLOYEE "1233"

typedef struct {
    int busId;
    char destination[50];
    int seatsAvailable;
    float ticketPrice;
} Bus;

typedef struct {
    int ticketId;
    int busId;
    int seatsReserved;
    float totalPrice;
} Ticket;

Bus buses[MAX_BUSES];
int busCount = 0;

Ticket tickets[MAX_TICKETS];
int ticketCount = 0;

void clearScreen();
void loadingAnimation();
void firstMenu();
void login();
void customerMenu();
void employeeMenu();
void addBus();
void removeBus();
void viewAllBuses();
void viewAllTickets();
void reserveSeat();
void cancelReservation();
void viewBookingHistory();
void displayDestinations();
int findBusById(int busId);
int findAvailableSeats(int busId);
int findTicketById(int ticketId);
void issueTicket(int busId, int seatsReserved);
void refundTicket(int ticketId, int seatsReserved);
void saveData();
void loadData();

int main() {
    loadingAnimation();
    loadData();
    firstMenu();
    return 0;
}

void clearScreen() {
    system("cls");
}
void loadingAnimation() {
    int i,delay=150000;
    for(i=0;i<20;i++){
        printf("\r\t\t\t\t");
        printf("please wait....LOADING");
        for(int j=0;j<i;j++){
          printf(">");
        }
       for(int j=i;j<19;j++){
          printf("_");
    }
    printf("%c","//-\\"[i%4]);
    fflush(stdout);
    usleep(delay);
    }
    printf("\n\t\t//^^^^\\|\n");
     printf("\t\t|| bus ||\n");
      printf("\t\t--        --\n");
       printf("\t~~~||----[]----[]----||~~~\n");
    printf("\n\t\t\tLoading complete!\n\n\n\n\t\t\t\t\t\tBARASA TRANSPORT SYSTEM\n\n\n\n\t\t\t\t\tpress any key to continue\n");
    getch();

}

void firstMenu() {
    int userType;
    do {
        clearScreen();
        printf("\t\t\t\t\t====WELCOME!!!=====\n\t\t\t\tBARASA TRASPORT COMPANY Ltd\n choose option 1 if customer and 2 if employee and login =====>\n");
        printf("1. Customer\n");
        printf("2. Employee\n");
        printf("3. Exit\n");
        printf("Select User Type: ");
        scanf("%d", &userType);

        switch (userType) {
            case 1:
                customerMenu();
                break;
            case 2:
                login();
                break;
            case 3:
                saveData();
                break;
            default:
                printf("Invalid choice. Please try again.\n");
                getchar(); // Wait for user to press Enter
        }
    } while (userType != 3);
}

void login() {
    char username[20];
    char password[5];

    clearScreen();
    printf("===== Employee Login =====\n");

    printf("Enter your username: ");
    scanf("%s", username);

    HANDLE hStdin = GetStdHandle(STD_INPUT_HANDLE);
    DWORD mode;
     printf("Enter your password: ");
    int i = 0;
    while (1) {
        char c = _getch(); // Read a character without echoing it
        if (c == '\r') {  // Enter key
            password[i] = '\0';
            break;
        } else if (c == '\b' && i > 0) { // Backspace
            i--;
            printf("\b \b"); // Erase the asterisk and move back
        } else if (i < sizeof(password) - 1) {
            password[i] = c;
            i++;
            printf("*"); // Print an asterisk
        }
    }

    if (strcmp(username, USERNAME_EMPLOYEE) == 0 && strcmp(password, PASSWORD_EMPLOYEE) == 0) {
        employeeMenu();
    } else {
        printf("\nInvalid username or password.\n");
        getchar(); // Wait for user to press Enter
    }
}



void customerMenu() {
    int choice;
    int selectedDestination;

    do {
        clearScreen();
        printf("===== Customer Menu =====\n");
        displayDestinations();

        printf("Select a destination (enter the corresponding number): ");
        scanf("%d", &selectedDestination);

        if (selectedDestination < 1 || selectedDestination > busCount) {
            printf("Invalid destination choice. Please try again.\n");
            getchar(); // Wait for user to press Enter
            continue;
        }

        // Display available buses and seats for the selected destination
        displayAvailableBusesForDestination(selectedDestination);

        printf("1. Reserve a Seat\n");
        printf("2. Return to Destination Selection\n");
        printf("3.  view ticket\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                reserveSeat(selectedDestination);
                break;
            case 2:
                // Return to destination selection
                break;
            case 3:
                viewAllTickets();
            case 4:
                saveData();
                break;
            default:
                printf("Invalid choice. Please try again.\n");
                getchar(); // Wait for user to press Enter
        }
    } while (choice != 4);
}


void employeeMenu() {
    int choice;
    do {
        clearScreen();
        printf("===== Employee Menu =====\n");
        printf("1. Add Bus\n");
        printf("2. Remove Bus\n");
        printf("3. View All Buses\n");
        printf("4. View All Tickets\n");
        printf("5. View Booking History\n");
        printf("6. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                addBus();
                break;
            case 2:
                removeBus();
                break;
            case 3:
                viewAllBuses();
                break;
            case 4:
                viewAllTickets();
                break;
            case 5:
                viewBookingHistory();
                break;
            case 6:
                saveData();
                break;
            default:
                printf("Invalid choice. Please try again.\n");
                getchar(); // Wait for user to press Enter
        }
    } while (choice != 6);
}


void addBus() {
    if (busCount < MAX_BUSES) {
        clearScreen();
        printf("===== Add Bus =====\n");

        Bus newBus;
        newBus.busId = busCount + 1;

        printf("Destination: ");
        scanf("%s", newBus.destination);
        printf("Seats Available: ");
        scanf("%d", &newBus.seatsAvailable);
        printf("Ticket Price: ");
        scanf("%f", &newBus.ticketPrice);

        buses[busCount++] = newBus;
        printf("Bus added successfully.\n");
        getchar(); // Wait for user to press Enter
    } else {
        printf("Maximum number of buses reached.\n");
        getchar(); // Wait for user to press Enter
    }
}

void removeBus() {
    clearScreen();
    printf("===== Remove Bus =====\n");
    viewAllBuses();

    int busId;
    printf("Enter Bus ID to remove: ");
    scanf("%d", &busId);

    int index = findBusById(busId);
    if (index != -1) {
        for (int i = index; i < busCount - 1; i++) {
            buses[i] = buses[i + 1];
        }
        busCount--;
        printf("Bus removed successfully.\n");
    } else {
        printf("Error: Bus not found.\n");
    }
    getchar(); // Wait for user to press Enter
}

void viewAllBuses() {
    clearScreen();
    printf("===== All Buses =====\n");
    printf("Bus ID\tDestination\tSeats Available\tTicket Price\n");

    for (int i = 0; i < busCount; i++) {
        printf("%d\t%s\t%d\t$%.2f\n", buses[i].busId, buses[i].destination, buses[i].seatsAvailable, buses[i].ticketPrice);
    }
    printf("\n");
    getchar(); // Wait for user to press Enter
}

void viewAllTickets() {
    clearScreen();
    printf("===== All Tickets =====\n");
    printf("Ticket ID\tBus ID\tSeats Reserved\tTotal Price\n");

    for (int i = 0; i < ticketCount; i++) {
        printf("%d\t%d\t%d\t$%.2f\n", tickets[i].ticketId, tickets[i].busId, tickets[i].seatsReserved, tickets[i].totalPrice);
    }
    printf("\n");
    getchar(); // Wait for user to press Enter
}


void displayAvailableBusesForDestination(int destinationChoice) {
    clearScreen();
    printf("===== Available Buses for Destination: %s =====\n", buses[destinationChoice - 1].destination);
    printf("Bus ID\tSeats Available\tTicket Price\n");

    for (int i = 0; i < busCount; i++) {
        if (i == destinationChoice - 1) {
            printf("%d\t%d\t$%.2f\n", buses[i].busId, buses[i].seatsAvailable, buses[i].ticketPrice);
        }
    }
    printf("\n");
    getchar(); // Wait for user to press Enter
}

void reserveSeat(int selectedDestination) {
    int busId, seatsToReserve;
    printf("Enter Bus ID to reserve a seat: ");
    scanf("%d", &busId);

    int busIndex = findBusById(busId);

    if (busIndex != -1 && busIndex == selectedDestination - 1) {
        printf("Enter the number of seats to reserve: ");
        scanf("%d", &seatsToReserve);

        if (seatsToReserve > 0 && seatsToReserve <= buses[busIndex].seatsAvailable) {
            // Check if there are enough available seats
            if (seatsToReserve <= findAvailableSeats(busId)) {
                // Issue the ticket and update available seats
                issueTicket(busId, seatsToReserve);
                printf("Reservation successful. Total price: $%.2f\n", buses[busIndex].ticketPrice * seatsToReserve);
            } else {
                printf("Error: Not enough available seats on this bus.\n");
            }
        } else {
            printf("Error: Invalid number of seats or not enough available seats.\n");
        }
        getchar(); // Wait for user to press Enter
    } else {
        printf("Error: Bus not found or not in the selected destination.\n");
        getchar(); // Wait for user to press Enter
    }
}

void cancelReservation() {
    clearScreen();
    printf("===== Cancel Reservation =====\n");
    viewAllTickets();

    int ticketId;
    printf("Enter Ticket ID to cancel: ");
    scanf("%d", &ticketId);

    int ticketIndex = findTicketById(ticketId);

    if (ticketIndex != -1) {
        refundTicket(ticketId, tickets[ticketIndex].seatsReserved);
        printf("Reservation canceled. Refund processed.\n");
        getchar(); // Wait for user to press Enter
    } else {
        printf("Ticket not found.\n");
        getchar(); // Wait for user to press Enter
    }
}

void viewBookingHistory() {
    clearScreen();
    printf("===== Booking History =====\n");
    printf("Feature not implemented in this example.\n");
    getchar(); // Wait for user to press Enter
}

void displayDestinations() {
    printf("Available Destinations: ");
    for (int i = 0; i < busCount; i++) {
        printf("%s", buses[i].destination);
        if (i < busCount - 1) {
            printf(", ");
        }
    }
    printf("\n");
    getchar(); // Wait for user to press Enter
}

int findBusById(int busId) {
    for (int i = 0; i < busCount; i++) {
        if (buses[i].busId == busId) {
            return i;
        }
    }
    return -1;
}

int findAvailableSeats(int busId) {
    int index = findBusById(busId);
    if (index != -1) {
        return buses[index].seatsAvailable;
    }
    return -1;
}

int findTicketById(int ticketId) {
    for (int i = 0; i < ticketCount; i++) {
        if (tickets[i].ticketId == ticketId) {
            return i;
        }
    }
    return -1;
}

void issueTicket(int busId, int seatsReserved) {
    int busIndex = findBusById(busId);
    if (busIndex != -1) {
        buses[busIndex].seatsAvailable -= seatsReserved;

        Ticket newTicket;
        newTicket.ticketId = ticketCount + 1;
        newTicket.busId = busId;
        newTicket.seatsReserved = seatsReserved;
        newTicket.totalPrice = buses[busIndex].ticketPrice * seatsReserved;

        tickets[ticketCount++] = newTicket;
    }
}

void refundTicket(int ticketId, int seatsReserved) {
    int ticketIndex = findTicketById(ticketId);
    if (ticketIndex != -1) {
        int busId = tickets[ticketIndex].busId;
        int busIndex = findBusById(busId);

        if (busIndex != -1) {
            buses[busIndex].seatsAvailable += seatsReserved;
            for (int i = ticketIndex; i < ticketCount - 1; i++) {
                tickets[i] = tickets[i + 1];
            }
            ticketCount--;
        }
    }
}

void saveData() {
    FILE *file = fopen("bus_data.txt", "w");
    if (file == NULL) {
        printf("Error: Could not open file for saving data.\n");
        return;
    }

    // Write bus data to the file
    for (int i = 0; i < busCount; i++) {
        fprintf(file, "%d %s %d %.2f\n", buses[i].busId, buses[i].destination, buses[i].seatsAvailable, buses[i].ticketPrice);
    }

    fclose(file);
    printf("Data saved.\n");
    exit(0);
}

void loadData() {
    FILE *file = fopen("bus_data.txt", "r");
    if (file == NULL) {
        printf("Error: Could not open file for loading data.\n");
        return;
    }

    // Read bus data from the file
    while (fscanf(file, "%d %s %d %f", &buses[busCount].busId, buses[busCount].destination, &buses[busCount].seatsAvailable, &buses[busCount].ticketPrice) == 4) {
        busCount++;
    }

    fclose(file);
    printf("Data loaded.\n");
}
