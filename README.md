# AICP-8
#include <iostream>
#include <vector>
#include <iomanip>

using namespace std;

// Constants
const int NUM_BOATS = 10;
const double HOURLY_RATE = 20.0;
const double HALF_HOUR_RATE = 12.0;
const int OPENING_HOUR = 10;
const int CLOSING_HOUR = 17;

// Structure to store boat information
struct Boat {
    int boatNumber;
    double moneyTaken;
    int totalHoursHired;
    int returnTime;  // in 24-hour format
};

// Function to calculate the money taken in a day for one boat (TASK 1)
void calculateDailyProfits(Boat& boat) {
    int startTime, hireDuration;
    
    cout << "Enter the start time for boat #" << boat.boatNumber << " (in 24-hour format): ";
    cin >> startTime;

    if (startTime < OPENING_HOUR || startTime > CLOSING_HOUR) {
        cout << "Boat cannot be hired before 10:00 or after 17:00.\n";
        return;
    }

    cout << "Enter the duration of hire (in hours): ";
    cin >> hireDuration;

    if (startTime + hireDuration > CLOSING_HOUR) {
        cout << "Boat must be returned before 17:00.\n";
        return;
    }

    double payment;
    if (hireDuration == 0.5) {
        payment = HALF_HOUR_RATE;
    } else {
        payment = HOURLY_RATE * hireDuration;
    }

    boat.moneyTaken += payment;
    boat.totalHoursHired += hireDuration;
    boat.returnTime = startTime + hireDuration;

    cout << fixed << setprecision(2);
    cout << "Boat #" << boat.boatNumber << " hired for " << hireDuration << " hours. Payment: $" << payment << endl;
}

// Function to find the next boat available (TASK 2)
void findNextAvailableBoat(const vector<Boat>& boats) {
    int currentTime;
    cout << "Enter the current time (in 24-hour format): ";
    cin >> currentTime;

    int earliestReturnTime = CLOSING_HOUR + 1;  // Initialize to a time after closing hour
    int availableBoats = 0;

    for (const auto& boat : boats) {
        if (boat.returnTime <= currentTime) {
            availableBoats++;
        } else {
            earliestReturnTime = min(earliestReturnTime, boat.returnTime);
        }
    }

    if (availableBoats > 0) {
        cout << "Number of boats available for hire: " << availableBoats << endl;
    } else {
        cout << "No boats available. Earliest available time: " << earliestReturnTime << ":00\n";
    }
}

// Function to calculate the money taken for all the boats at the end of the day (TASK 3)
void calculateTotalProfits(const vector<Boat>& boats) {
    double totalMoneyTaken = 0;
    int totalHoursHired = 0;
    int unusedBoats = 0;
    int mostUsedBoat = -1;
    int maxHoursHired = -1;

    for (const auto& boat : boats) {
        totalMoneyTaken += boat.moneyTaken;
        totalHoursHired += boat.totalHoursHired;

        if (boat.totalHoursHired == 0) {
            unusedBoats++;
        }

        if (boat.totalHoursHired > maxHoursHired) {
            maxHoursHired = boat.totalHoursHired;
            mostUsedBoat = boat.boatNumber;
        }
    }

    cout << "\nEnd of day report:\n";
    cout << "Total money taken: $" << totalMoneyTaken << endl;
    cout << "Total number of hours boats were hired: " << totalHoursHired << " hours\n";
    cout << "Number of boats not used: " << unusedBoats << endl;

    if (mostUsedBoat != -1) {
        cout << "Boat #" << mostUsedBoat << " was used the most with " << maxHoursHired << " hours hired.\n";
    }
}

int main() {
    vector<Boat> boats(NUM_BOATS);

    // Initialize boat data
    for (int i = 0; i < NUM_BOATS; ++i) {
        boats[i].boatNumber = i + 1;
        boats[i].moneyTaken = 0.0;
        boats[i].totalHoursHired = 0;
        boats[i].returnTime = OPENING_HOUR;  // Initialize to opening hour
    }

    // Task 1 - Calculate the money taken in a day for one boat
    for (int i = 0; i < NUM_BOATS; ++i) {
        calculateDailyProfits(boats[i]);
    }

    // Task 2 - Find the next boat available
    findNextAvailableBoat(boats);

    // Task 3 - Calculate the money taken for all the boats at the end of the day
    calculateTotalProfits(boats);

    return 0;
}
