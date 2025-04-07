# Revision

## DATA REQUIREMENTS
Diagrams Needed:

Data Dictionary

Enitity-Relationship Diagram

Data Flow Diagram


MUST INCLUDE ERROR HANDLING DETAILS

#### Error Handling:

Data Validation
Type Casting
Maximum Field Length
Feedback to users/error messages

#### Algorithm Designs

Must talk about Key Processes like the following:

Handling user access (Accounts, Changing Passwords)

Creation and Processing of data (inputting data, storing data, changing information)

Communication/data exchange (passing things between front and back end, e.g. displaying items from databases)

##### Algorithms

Many algorithms needed. Will be 1 for each main section of the site. 

Expected Features:

Clearly Defined Steps

Clear Logic

Efficiency, no unnecessary steps or loops

Modularity, allowing the algorithm to be reused.

#### Test Table

Unit Testing

Integration Testing

System Testing

User Acceptance Testing
-----------------------------------------------------
Clearly define what each component is and what it is responsible for

Explain how the component interacts with other parts of the solution

Specific Testing Techniques:

Functional + Non-functional Testing

Regression Testing

Security Testing

#### Legislation

Regulatory Guidelines: Discuss how your solution will adhere to relevant regulatory guidelines and legal requirements for the industry or context. 

Privacy Concerns: How will you protect user data in the context of the scenario? 

User Errors: What measures will you implement to prevent user mistakes, particularly for vulnerable user groups? 

Intellectual Property: Address how you will ensure the proper use of any content or materials relevant to the occupational scenario. 


## Code


### Error Handling

       private void btnCalculate_Click(object sender, EventArgs e) 

        { 

            try 

            { 

                int tickets = int.Parse(txtTickets.Text); 

                if (tickets < 0 )  

                { 

                    MessageBox.Show("Invalid input! Please enter a positive number.", "Format Error", MessageBoxButtons.OK, MessageBoxIcon.Error); 

                } 

                else 

                { 

                    double answer = (tickets * 10.99); 

                    txtAnswer.Text = answer.ToString(); 

                    if (answer > 40000 )  

                    { 

                        MessageBox.Show("Ticket sales are high"); 

                    } 

                    else  

                    { 

                        MessageBox.Show("Ticket sales are low"); 

                    } 

                }
                
            } 

            catch (FormatException f) 

            { 

                MessageBox.Show("Invalid input! Please enter numeric values.", "Format Error", MessageBoxButtons.OK, MessageBoxIcon.Error); 

            } 

            catch (Exception ex) 

            { 
            
                MessageBox.Show($"Unexpected error: {ex.Message}", "Error", MessageBoxButtons.OK, MessageBoxIcon.Error); 

            } 

        } 

    } 
    
} 

### Account Creation

string connectionString = "Data Source = (LocalDB)\\MSSQLLocalDB; AttachDbFilename = C:\\Users\\MC03286\\OneDrive - Middlesbrough College\\T-Level C#\\MyDBTest\\MyDB.mdf; Integrated Security = True; Connect Timeout = 30";

SqlConnection sqlConnection = new SqlConnection(connectionString);

//Use stored procedure

SqlCommand command = new SqlCommand("CreateNewPersonRecord", sqlConnection);

command.CommandType = CommandType.StoredProcedure;

//Input name and age from form

string name = txtName.Text;

int age = int.Parse(txtAge.Text);

//Call stored procedure passing name and age as parameters

command.Parameters.AddWithValue("@Name", name);

command.Parameters.AddWithValue("@Age", age);

//Open connection to database, execute stored procedure and close the connection

sqlConnection.Open();

command.ExecuteNonQuery();

sqlConnection.Close();

##### Stored Procedure

CREATE procedure [dbo].[CreateNewPersonRecord]

(

@Name nvarchar (50),

@Age int

)

as

begin

Insert into People values(@Name,@Age)

End

#### Read Data

private void btnRead_Click(object sender, EventArgs e)

{

//Read a person record

string connectionString = "Data Source = (LocalDB)\\MSSQLLocalDB; AttachDbFilename = C:\\Users\\MC03286\\OneDrive - Middlesbrough College\\T-Level C#\\2024-25\\DBExampleVB2019\\DBEgVB2019.mdf; Integrated Security = True; Connect Timeout = 30";

SqlConnection sqlConnection = new SqlConnection(connectionString);

SqlCommand cmd = new SqlCommand("GetPersonDetails", sqlConnection);

cmd.CommandType = CommandType.StoredProcedure;

SqlDataAdapter sd = new SqlDataAdapter(cmd);

DataTable dt = new DataTable();

sqlConnection.Open();

sd.Fill(dt);

sqlConnection.Close();

//Read rows of database and extract fields

foreach (DataRow dr in dt.Rows)

{

int personId = (int)(dr["Id"]);

string personName = (string)(dr["Name"]);

int personAge = (int)(dr["Age"]);

txtPeople.AppendText(personName + "\t" + personAge.ToString() + Environment.NewLine);

}

}

##### Stored Procedure

Create Procedure [dbo].[GetPersonDetails]

as

begin

select * from People

End

##### Class

class Person

{


public int Id { get; set; }

public string Name { get; set; }


public int Age { get; set; }

}

#### Update

private void btnEdit_Click(object sender, EventArgs e)

{

string connectionString = "Data Source = (LocalDB)\\MSSQLLocalDB; AttachDbFilename = C:\\Users\\MC03286\\OneDrive - Middlesbrough College\\PeopleDB2024\\PeopleDB2024.mdf; Integrated Security = True; Connect Timeout = 30";

SqlConnection sqlConnection = new SqlConnection(connectionString);

//USE STORED PROCEDURE

SqlCommand command = new SqlCommand("UpdatePerson", sqlConnection);

command.CommandType = CommandType.StoredProcedure;

string name = txtEditName.Text;

int age = int.Parse(txtEditAge.Text);

command.Parameters.AddWithValue("@StdID", currentID);

command.Parameters.AddWithValue("@Name", name);

command.Parameters.AddWithValue("@Age", age);

sqlConnection.Open();

command.ExecuteNonQuery();

sqlConnection.Close();

}

##### Stored Procedure

Create procedure [dbo].[UpdatePerson]

(

@StdId int,

@Name nvarchar (50),

@Age int

)

as

begin

Update Person

set Name=@Name,

Age=@Age

where Id=@StdId

End
