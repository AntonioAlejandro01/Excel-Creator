# Excel-Creator

Easily generate an excel file

## How Implement Interface

Implement this interface ``` IExcelObject ``` with a method:

- ``` obtainFields() ```: return a List of ExcelData Object in the same order that introduce the headers values.  

### ExcelData Object

This Object have a overloaded constructor. The values that can accept are strings, Integers, Long, Double, Date and boolean. The boolean data can insert custom string values (true and false are default values) like an example with gender.

```java
public class Example implements IExcelObject{

    private String name;
    private String lastname;
    private Integer age;
    private boolean isMan;

    public Ejemplo(String name, String lastname, Integer age, boolean isMan){
        super();
        this.name = name;
        this.lastname = lastname;
        this.age = age;
        this.isMan = isMan;
    }


    public List<ExcelData> obtainFields(){
        return Arrays.asList(
                    new ExcelData(name),
                    new ExcelData(lastname),
                    new ExcelData(age),
                    new ExcelData(isMan,"Man","Woman"));
    }
}
```

## How create a Excel File

### Create a ExcelBook Object

The constructor are overloaded. You can put the sheetname only, the sheetname and headers or sheetname,headers, and the list object(excel data).

```java
public static void main(String[] args) {
    //the data
    List<Ejemplo> data = Arrays.asList(new Ejemplo("Name 1","Lastname 1", 10, true),new Ejemplo("Name 2","Lastname 2", 20, false));
    ExcelBook<Ejemplo> excelBook = new ExcelBook<Ejemplo>("Example");
    // Set headers values
    excelBook.setHeaders(Ejemplo.getHeaders());
    // Set Data
    excelBook.setHeaders(data);
    // you can set color to headers row use java.awt.Color object
    excelBook.setHeaderColor(new Color(125,125,125));
    // you can set color to data cells
    excelBook.setDataColor(new Color(255,255,255));
    // you can remove cells borders
    excelBook.setBlankSheet(true);
    //  if the excel contain dates is Data you can set a format. the formats are specified in ExcelCellDateFormat enum
    excelBook.setFormatDate(ExcelCellDateFormat.NUMBER_SHORT_WITH_TIME);
    // you can set a imagen like logo. you add image like byte[] or Input Stream
    excelBook.addLogo(/*byte[] or inputStream*/);
    // you have to close the Excelbook to create safely
    excelBook.close(); // throws IOException
    // when you are to prepare to send file o write file in pc. you have two options
    // write file in yout pc
    try{
        excelBook.write("path in your computer"/*Example: /home/user/Desktop/ejemplo(name file without extension)*/);
    }catch(IOException e){}
    //or recive a byte[]
    try{
        byte[] bytes = excelbook.prepareToSend();
    }catch(IOException e){}
}
```

## Create your own ExcelBook

The class ExcelBook is a predifiend implementation. So you can extends ExcelBookAbstract to create your own custom class.
