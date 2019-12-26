Developer Guide
===============
For EmployeeDB, I have worked on the tables JOBTITLES, LEVEL, WORKCHART
and their related functions.

Database Design
---------------
* The "JOBTITLES" table contains the available job titles along with their properties like its department, whether it is active, executive, or can be hired.
* The "LEVEL" table contains the available levels for jobs along with their properties like its experience year, bonus salary, whether it is director or manager.
* The "WORKCHART" table contains the current workchart information which maps the name of the employee to their job title, level, salary, foodbudget, total years they worked, years in the company and whether they quailfy for pension.

**include the E/R diagram(s)**

Code
----
Database Functions
~~~~~~~~~~~~~~~~~~
::
def add_jobtitle(self, jobtitle):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "INSERT INTO JOBTITLES (JOBNAME, IS_EXECUTIVE, DEPARTMENT, IS_ACTIVE, TO_BE_HIRED) VALUES (%s, %s, %s,%s,%s)"
            cursor.execute(query, (jobtitle.title, jobtitle.is_executive,jobtitle.department, jobtitle.is_active, jobtitle.to_be_hired))
            connection.commit()
            jobtitle_key = cursor.lastrowid
        return jobtitle_key

    def delete_jobtitle(self, jobtitle_key):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "DELETE FROM JOBTITLES WHERE (ID = %s)"
            cursor.execute(query, (jobtitle_key,))
            connection.commit()

    def update_jobtitle(self, jobtitle_key, title, is_executive, department, is_active, to_be_hired):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "UPDATE JOBTITLES SET JOBNAME = %s, IS_EXECUTIVE = %s, DEPARTMENT = %s, IS_ACTIVE = %s, TO_BE_HIRED= %s WHERE (ID = %s)"
            cursor.execute(query, (title, is_executive, department, is_active, to_be_hired, jobtitle_key))
            connection.commit()
        return jobtitle_key
    
    def get_jobtitle(self, jobtitle_key):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "SELECT JOBNAME, IS_EXECUTIVE, DEPARTMENT, IS_ACTIVE, TO_BE_HIRED FROM JOBTITLES WHERE (ID = %s)"
            cursor.execute(query, (jobtitle_key,))
            title,is_executive,department,is_active,to_be_hired = cursor.fetchone()
        jobtitle_ = Jobtitle(title,is_executive,department,is_active,to_be_hired)
        return jobtitle_
    
    def get_jobtitles(self):
        jobtitles = []
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "SELECT ID, JOBNAME, IS_EXECUTIVE, DEPARTMENT, IS_ACTIVE, TO_BE_HIRED FROM JOBTITLES ORDER BY ID"
            cursor.execute(query)
            for jobtitle_key, title, is_executive, department, is_active, to_be_hired in cursor:
                jobtitles.append((jobtitle_key, Jobtitle(title,is_executive,department,is_active,to_be_hired)))
        return jobtitles

    def get_jobtitle_id(self, jobtitle_name):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "SELECT ID FROM JOBTITLES WHERE (JOBNAME = %s)"
            cursor.execute(query, (jobtitle_name,))
            jobtitle_id = cursor.fetchone()
        return jobtitle_id    

These are the functions about job titles, for all CRUD operations as well as getting the job title id for the foreign key operations.

::
   def add_level(self, level):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "INSERT INTO LEVEL (LEVELNAME, EXPERIENCE_YEAR_NEEDED, BONUS_SALARY, IS_DIRECTOR, IS_MANAGER) VALUES (%s, %s, %s, %s, %s);"
            cursor.execute(query, (level.title, level.experience, level.bonus_salary, level.is_director, level.is_manager))
            connection.commit()
            level_key = cursor.lastrowid
        return level_key

    def delete_level(self, level_key):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "DELETE FROM LEVEL WHERE (ID = %s)"
            cursor.execute(query, (level_key,))
            connection.commit()

    def update_level(self, level_key, title, experience, bonus_salary, is_director, is_manager):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "UPDATE LEVEL SET LEVELNAME = %s, EXPERIENCE_YEAR_NEEDED = %s, BONUS_SALARY = %s, IS_DIRECTOR = %s, IS_MANAGER= %s WHERE (ID = %s)"
            cursor.execute(query, (title, experience, bonus_salary, is_director, is_manager,level_key))
            connection.commit()
        return level_key
    
    def get_level(self, level_key):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "SELECT LEVELNAME, EXPERIENCE_YEAR_NEEDED, BONUS_SALARY, IS_DIRECTOR, IS_MANAGER FROM LEVEL WHERE (ID = %s)"
            cursor.execute(query, (level_key,))
            title, experience, bonus_salary, is_director, is_manager = cursor.fetchone()
        level_ = Level(title, experience, bonus_salary, is_director, is_manager)
        return level_
    
    def get_levels(self):
        levels = []
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "SELECT ID, LEVELNAME, EXPERIENCE_YEAR_NEEDED, BONUS_SALARY, IS_DIRECTOR, IS_MANAGER FROM LEVEL ORDER BY ID"
            cursor.execute(query)
            for level_key, title, experience, bonus_salary, is_director, is_manager in cursor:
                levels.append((level_key, Level(title, experience, bonus_salary, is_director, is_manager)))
        return levels
    
    def get_level_id(self, level_name):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "SELECT ID FROM LEVEL WHERE (LEVELNAME = %s)"
            cursor.execute(query, (level_name,))
            level_id = cursor.fetchone()
        return level_id  

These are the functions about level, for all CRUD operations as well as getting the level id for the foreign key operations.

::
def add_workchart(self, workchart):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "INSERT INTO WORKCHART (PERSONID, JOBID, LEVELID, SALARY, FOOD_BUDGET, TOTAL_YEARS_WORKED, YEARS_IN_COMPANY, QUALIFIES_FOR_PENSION) VALUES (%s, %s, %s, %s, %s, %s, %s, %s)"
            cursor.execute(query, (workchart.personid, workchart.jobid, workchart.levelid, workchart.salary, workchart.foodbudget, workchart.total_yr_worked, workchart.yr_in_comp, workchart.qualify))
            connection.commit()
            workchart_key = workchart.personid
        return workchart_key

    def delete_workchart(self, workchart_key):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "DELETE FROM WORKCHART WHERE (PERSONID = %s)"
            cursor.execute(query, (workchart_key,))
            connection.commit()

    def get_workchart(self, workchart_key):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "SELECT PERSONID, JOBID, LEVELID, SALARY, FOOD_BUDGET, TOTAL_YEARS_WORKED, YEARS_IN_COMPANY, QUALIFIES_FOR_PENSION FROM WORKCHART WHERE (PERSONID = %s)"
            cursor.execute(query, (workchart_key,))
            personid, jobid, levelid, salary, foodbudget, total_yr_worked, yr_in_comp, qualify = cursor.fetchone()
        workchart_ = Workchart(personid, jobid, levelid, salary, foodbudget, total_yr_worked, yr_in_comp, qualify)
        return workchart_

    def get_workcharts(self):
        workcharts = []
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "SELECT PERSONID, JOBID, LEVELID, SALARY, FOOD_BUDGET, TOTAL_YEARS_WORKED, YEARS_IN_COMPANY, QUALIFIES_FOR_PENSION FROM WORKCHART ORDER BY SALARY DESC"
            cursor.execute(query)
            for personid, jobid, levelid, salary, foodbudget, total_yr_worked, yr_in_comp, qualify in cursor:
                workcharts.append((personid, Workchart(personid, jobid, levelid, salary, foodbudget, total_yr_worked, yr_in_comp, qualify)))
        return workcharts
    
    def update_workchart(self, personid, jobid, levelid, salary, foodbudget, total_yr_worked, yr_in_comp, qualify):
        with dbapi2.connect(self.dbfile) as connection:
            cursor = connection.cursor()
            query = "UPDATE WORKCHART SET JOBID = %s, LEVELID = %s, SALARY = %s, FOOD_BUDGET = %s, TOTAL_YEARS_WORKED= %s, YEARS_IN_COMPANY = %s, QUALIFIES_FOR_PENSION = %s WHERE (PERSONID = %s)"
            cursor.execute(query, (jobid, levelid, salary, foodbudget, total_yr_worked, yr_in_comp, qualify, personid))
            connection.commit()
        return personid
    
These are the functions about workchart, for all CRUD operations as well as getting the names of employees, titles of jobs, titles of levels from their id values from the foreign key operations.



.. toctree::

   member1
   member2