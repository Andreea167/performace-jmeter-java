# Performance Testing with JMeter (CI Integrated)

This project contains a JMeter test plan for testing the [Swagger Petstore API](https://petstore.swagger.io/). The test plan is defined in [`API_Testing.jmx`](API_Testing.jmx) and uses data from [`pet_data.csv`](pet_data.csv), instegrated into CI.

## Project Structure

- [`API_Testing.jmx`](API_Testing.jmx): The main JMeter test plan.
- [`pet_data.csv`](pet_data.csv): CSV file with pet data (name, status, photoUrl).
- [`.github/workflows/jmeter.yml`](.github/workflows/jmeter.yml): GitHub Actions workflow for running JMeter tests in CI.

## Test Plan Overview

The test plan performs the following actions:

1. **Reads pet data from CSV**  
   - Uses `pet_data.csv` as a data source.
   - Variables: `name`, `status`, `photoUrl`.

2. **HTTP Header Manager**  
   - Sets `Content-Type: application/json` for requests.

3. **Add Pet (POST /v2/pet)**  
   - Sends a POST request to add a new pet using data from the CSV.
   - Generates a random `petId` for each request.

4. **Extract petId**  
   - Extracts the `id` from the response and stores it as `${petId}` for later use.

5. **Get Pet (GET /v2/pet/${petId})**  
   - Retrieves the pet by its ID.

6. **Update Pet (PUT /v2/pet)**  
   - Updates the pet's name (appends `-updated`).

7. **Find Pets by Status (GET /v2/pet/findByStatus?status=pending)**  
   - Retrieves pets with status `pending`.

8. **Delete Pet (DELETE /v2/pet/${petId})**  
   - Deletes the pet by its ID.

9. **Assertions**  
   - Each HTTP request checks for a `200` response code.

10. **Timers**  
    - Think times and random pauses are added between requests.

11. **Reports**  
    - Includes "View Results Tree" and "Summary Report" listeners.

## Thread Group Configuration

- **Number of Threads:** 1
- **Ramp-Up Period:** 1 second
- **Loop Count:** 1

## How to Run

1. Open [`API_Testing.jmx`](API_Testing.jmx) in JMeter.
2. Ensure [`pet_data.csv`](pet_data.csv) is in the same directory as the JMX file.
3. Run the test plan.

## Notes

- The test plan uses both the CSV Data Set Config and a Groovy script to read random lines from the CSV. You may want to remove one of these methods to avoid conflicts.
- Make sure JMeter has access to the required libraries for JSON processing (Jackson) if running the Java post