# Entity Framework C# Stored Procedure Model Binding
These classes can help you to do easier the execution of store Stored Procedure and binding it to a model.

#How it's working?
The Stored Procedure is executed with a `dbContext.Database.Connection.CreateCommand()` `CommandText`
For that, you have need 3 values (2 requireds and 1 optional).

First one, you need get your `DataBaseContext` Instance.
This classes are working with EntityFramework DataBaseContext, but you can use it without EntityFramework without problems.
Second one, you need assign the `StoreProcedure` name, this will used as reference to executed.
The third one, will be the parameters. You can assigne any of the `SqlDbType` you can get more reference about them here: https://msdn.microsoft.com/en-us/library/system.data.sqldbtype%28v=vs.110%29.aspx and you can add it to a list of the instance `Generic.cs`.

**Generic.cs** get 3 values. `Key`, `Value` and `Type`. If you need send a parameter to your store procedure like:

`var _generic = new List<Generic>`

                          {
                          
                              new Generic
                              
                                  {
                                  
                                      Key = "@phase", Type = SqlDbType.Int, Value = "207"
                                      
                                  }
                                  
                            };`
                            
The values are passed as string, but the reference of your type should be assigned in the `Type`.

The classes are usign reflection to search the column names, reference to the values and references to your `class`
More reference about reflection here: https://msdn.microsoft.com/en-us/library/f7ykdhsy%28v=vs.110%29.aspx
Thats means, you need have ***exactly the same name in your model properties to your store procedure column names***

If your store procedure returns:

`SELECT a.Name, b.Lastname, c.Person_Birthdate as Birthdate ....`
the properties in your Model should be:
`public string Name {get;set;}`
`public string Lastname {get;set;}`
`public DateTime Birthdate {get;set;}`

Excactly the same names and the model will be the encharged to bind your variable.

#Example of usage
`private List<Generic> _generic;

_generic = new List<Generic>
                          {
                          
                              new Generic
                              
                                  {
                                  
                                      Key = "@phase", Type = SqlDbType.Int, Value = "207"
                                      
                                  },
                                  
                              new Generic
                              
                                  {
                                  
                                      Key = "@filter", Type = SqlDbType.Text, Value = "2"
                                      
                                  }
                                  
                          };`
                          
`var result = DataReaderT.ReadStoredProceadures<ListaSolicitudModel>(entityModel, "dbo.RequestedData", _generic);`

Now your variable `result` has all the values in a `List<ListaSolicitudModel>` with all the properties of your list, like:
`result.Count()`

`result.First().Name //Possible null pointer exception, you need verify if result.Any() == true` 

#FAQ
1. **Can the class execute Stored Proceadures?** Yes, the principal functionallity of the class is execute stored proceadures with SQL Server, Oracle DB, MySql and Posgresql (relational databases).
2. **Can the class exceute Non-Query and Escalar?** Yes. It's possible, Also, if you are inserting values and generating the ID's from the table or with a trigger and with the execution, are you returning the id or some other data, will be biding in your model.
3. **Which what databases was tested?** Oracle DB, SQL Server, MySQL, Posgresql.
