# plsql-collections-records-goto-demo
Problem Statement: Electric Scooter Charging Station Monitoring
We have a task to building a PL/SQL procedure for Electric Scooter Company to monitor the status of multiple electric scooter charging stations. Each station has a unique ID, location, and a status indicating whether it's Active, Inactive, or Faulty. The system should:
•	Use a collection to store multiple charging station records.
•	Use a record to define the structure of each station.
•	Use a GOTO statement to skip faulty stations during processing.
Concepts Demonstrated
•	PL/SQL Collections: Using TABLE OF to store multiple station records.
•	PL/SQL Records: Defining a custom record type for station details.
•	GOTO Statements: Skipping faulty stations during iteration.
PL/SQL Code 
DECLARE
  -- Define a record type for a charging station
  TYPE station_rec IS RECORD (
    station_id   NUMBER,
    location     VARCHAR2(50),
    status       VARCHAR2(10)  -- 'Active', 'Inactive', 'Faulty'
  );
  -- Define a collection type of charging stations
  TYPE station_table IS TABLE OF station_rec INDEX BY BINARY_INTEGER;

  -- Declare the collection
  stations station_table;

  -- Loop counter
  i INTEGER := 1;

BEGIN
  -- Populate the collection with sample data
  stations(1) := station_rec(101, 'Kigali - Gasabo', 'Active');
  stations(2) := station_rec(102, 'Kigali - Kicukiro', 'Faulty');
  stations(3) := station_rec(103, 'Musanze', 'Inactive');
  stations(4) := station_rec(104, 'Huye', 'Active');
  -- Process each station
  WHILE i <= stations.COUNT LOOP
    IF stations(i).status = 'Faulty' THEN
      GOTO skip_station;
    END IF;
    DBMS_OUTPUT.PUT_LINE('Processing Station ' || stations(i).station_id || 
                         ' at ' || stations(i).location || 
                         ' - Status: ' || stations(i).status);
    <<skip_station>>
    i := i + 1;
  END LOOP;
END;

Expected Output 
Processing Station 101 at Kigali - Gasabo - Status: Active
Processing Station 103 at Musanze - Status: Inactive
Processing Station 104 at Huye - Status: Active
