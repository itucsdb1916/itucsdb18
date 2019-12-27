Parts Implemented by Hakan Saraç
================================
For EmployeeDB, I have worked on the tables JOBTITLES, LEVEL, WORKCHART
and their related functions.

Database Design
---------------
* The "PERSON" table contains the available person along with their properties like their name, age, gender, height, and weight.
* The "SERVICE" table contains the available services for employees along with their properties like its town, capacity, current passenger amount, licence plate and departure hour.
* The "TRANSPORTATION" table contains the current transportation information which maps the name of the employee to their services, seat number, service fee, stop name, whether they use it in the morning or evening.

**include the E/R diagram(s)**

Code
----
Database Functions
~~~~~~~~~~~~~~~~~~
    def add_employee(self, employee):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "INSERT INTO PERSON (NAME, AGE, GENDER, HEIGHT, WEIGHT) VALUES (%s, %s, %s, %s, %s)"
            cursor.execute(query, (employee.name, employee.age, employee.gender, employee.height, employee.weight))
            connection.commit()
            employee_key = cursor.lastrowid
        return employee_key

    def delete_employee(self, employee_key):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "DELETE FROM PERSON WHERE (ID = %s)"
            cursor.execute(query, (employee_key,))
            connection.commit()

    def update_employee(self, employee_key, name, age, gender, height, weight):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "UPDATE PERSON SET NAME = %s, AGE = %s, GENDER = %s, HEIGHT = %s, WEIGHT = %s WHERE (ID = %s)"
            cursor.execute(query, (name, age, gender, height, weight, employee_key))
            connection.commit()
        return employee_key

    def get_employee(self, employee_key):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "SELECT NAME, AGE, GENDER, HEIGHT, WEIGHT FROM PERSON WHERE (ID = %s)"
            cursor.execute(query, (employee_key,))
            name, age, gender, height, weight = cursor.fetchone()
        employee_ = Employee(name, age, gender, height, weight)
        return employee_

    def get_employees(self):
        employees = []
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "SELECT ID, NAME, AGE, GENDER, HEIGHT, WEIGHT FROM PERSON ORDER BY ID"
            cursor.execute(query)
            for employee_key, name, age, gender, height, weight in cursor:
                employees.append((employee_key, Employee(name, age, gender, height, weight)))
        return employees
    
    def get_employee_id(self, employee_name):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "SELECT ID FROM PERSON WHERE (NAME = %s)"
            cursor.execute(query, (employee_name,))
            employee_id = cursor.fetchone()
        return employee_id

* These are the functions about employees, for all CRUD operations as well as getting the employee id for the foreign key operations.

    def add_service(self, service):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "INSERT INTO SERVICE (TOWN, CAPACITY, CURRENT_PASSENGERS, LICENCE_PLATE, DEPARTURE_HOUR) VALUES (%s, %s, %s,%s,%s)"
            cursor.execute(query, (service.town, service.capacity, service.current_passengers, service.licence_plate,service.departure_hour))
            connection.commit()
            service_key = cursor.lastrowid
        return service_key

    def delete_service(self, service_key):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "DELETE FROM SERVICE WHERE (ID = %s)"
            cursor.execute(query, (service_key,))
            connection.commit()

    def update_service(self, service_key, town, capacity, current_passengers, licence_plate, departure_hour):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "UPDATE SERVICE SET TOWN = %s, CAPACITY = %s, CURRENT_PASSENGERS = %s, LICENCE_PLATE = %s, DEPARTURE_HOUR= %s WHERE (ID = %s)"
            cursor.execute(query, (town, capacity, current_passengers, licence_plate, departure_hour, service_key))
            connection.commit()
        return service_key
    
    def get_service(self, service_key):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "SELECT TOWN, CAPACITY, CURRENT_PASSENGERS, LICENCE_PLATE, DEPARTURE_HOUR FROM SERVICE WHERE (ID = %s)"
            cursor.execute(query, (service_key,))
            town,capacity,current_passengers,licence_plate,departure_hour = cursor.fetchone()
        service_ = Service(town,capacity,current_passengers,licence_plate,departure_hour)
        return service_
    
    def get_services(self):
        services = []
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "SELECT ID, TOWN, CAPACITY, CURRENT_PASSENGERS, LICENCE_PLATE, DEPARTURE_HOUR FROM SERVICE ORDER BY ID"
            cursor.execute(query)
            for service_key,town,capacity,current_passengers,licence_plate,departure_hour in cursor:
                services.append((service_key, Service(town,capacity,current_passengers,licence_plate,departure_hour)))
        return services

    def get_service_id(self, service_name):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "SELECT ID FROM SERVICE WHERE (TOWN = %s)"
            cursor.execute(query, (service_name,))
            service_id = cursor.fetchone()
        return service_id  

* These are the functions about services, for all CRUD operations as well as getting the service id for the foreign key operations.

    def add_transportation(self, transportation):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "INSERT INTO TRANSPORTATION (PERSONID, SERVICEID, USES_IN_MORNING, USES_IN_EVENING, SEAT_NUMBER, SERVICE_FEE, STOP_NAME) VALUES (%s, %s, %s, %s, %s, %s, %s)"
            cursor.execute(query, (transportation.personid, transportation.serviceid, transportation.uses_in_morning, transportation.uses_in_evening, transportation.seat_nr, transportation.service_fee, transportation.stop_name))
            connection.commit()
            transportation_key = transportation.personid
        return transportation_key

    def delete_transportation(self, transportation_key):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "DELETE FROM TRANSPORTATION WHERE (PERSONID = %s)"
            cursor.execute(query, (transportation_key,))
            connection.commit()

    def get_transportation(self, transportation_key):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "SELECT PERSONID, SERVICEID, USES_IN_MORNING, USES_IN_EVENING, SEAT_NUMBER, SERVICE_FEE, STOP_NAME FROM TRANSPORTATION WHERE (PERSONID = %s)"
            cursor.execute(query, (transportation_key,))
            personid,serviceid, uses_in_morning, uses_in_evening, seat_nr, service_fee, stop_name = cursor.fetchone()
        transportation_ = Transportation(personid,serviceid, uses_in_morning, uses_in_evening, seat_nr, service_fee, stop_name)
        return transportation_

    def get_transportations(self):
        transportations = []
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "SELECT PERSONID, SERVICEID, USES_IN_MORNING, USES_IN_EVENING, SEAT_NUMBER, SERVICE_FEE, STOP_NAME FROM TRANSPORTATION ORDER BY SERVICEID"
            cursor.execute(query)
            for personid,serviceid, uses_in_morning, uses_in_evening, seat_nr, service_fee, stop_name in cursor:
                transportations.append((personid, Transportation(personid,serviceid, uses_in_morning, uses_in_evening, seat_nr, service_fee, stop_name)))
        return transportations

    def update_transportation(self, personid, serviceid, uses_in_morning, uses_in_evening, seat_nr, service_fee, stop_name):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "UPDATE TRANSPORTATION SET SERVICEID = %s, USES_IN_MORNING = %s, USES_IN_EVENING = %s, SEAT_NUMBER = %s, SERVICE_FEE= %s, STOP_NAME = %s WHERE (PERSONID = %s)"
            cursor.execute(query, (serviceid, uses_in_morning, uses_in_evening, seat_nr, service_fee, stop_name, personid))
            connection.commit()
        return personid

* These are the functions about transportation, for all CRUD operations as well as getting the names of employees and towns of services from their id values from the foreign key operations.
