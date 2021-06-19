# XSS Exploitation and understanding

XSS or cross site scripting vulnerability is pretty much easy to find through blackbox approach and exploitation or bypass might require source code.  the root cause of XSS is simple where user input directly sends to the server response without sanitization. user input can be in multiple forms such as.

1) user used search functionality and response has the word seached by user ==> Reflacted XSS
2) user used comment functionality and that comment will be in response forever. ==> Stored XSS
3) user used JS functions used by target website and by abusing JavaScript injected JS code. ==> DOM based XSS

those are 3 major exploitation of XSS vulnerability. vulerability is not something which is critical itself however, by chaining it with other vulnerability this can be a total dammange to the target and can be a critical **categorized**

For our course we just need to understand XSS basics and exploitaions. for later topics we will be chaining with other vulnerability to get more impact over target.


### Reflacted XSS

#### PHP
```php
<?php
if( array_key_exists( "name", $_GET ) && $_GET[ 'name' ] != NULL ) {
    // Feedback for end user
    echo '<pre>Hello ' . $_GET[ 'name' ] . '</pre>';
}?>
```

Over above code as you can see line 2 verifies user input with `GET` parameter `name`. and line 4 echo's data which is passed through that `name` parameter inside `<pre>` tag. here user input is directly passed to response of website and any data will be passed to the reponse.

```php
<?php
if( array_key_exists( "name", $_GET ) && $_GET[ 'name' ] != NULL ) {
    // Get input
    $name = str_replace( '<script>', '', $_GET[ 'name' ] );

    // Feedback for end user
    echo "<pre>Hello ${name}</pre>";
}

?>
```

above code removes `<script>` tag using a function `str_replace()`. however, there are many ways to execute javascript without that script tag so here `security breaks`

```php
<?php
if( array_key_exists( "name", $_GET ) && $_GET[ 'name' ] != NULL ) {
    // Get input
    $name = preg_replace( '/<(.*)s(.*)c(.*)r(.*)i(.*)p(.*)t/i', '', $_GET[ 'name' ] );

    // Feedback for end user
    echo "<pre>Hello ${name}</pre>";
}

?>
```

Same vulnerable code which just replace scripts by checking indevidually.

```php
<?php

// Is there any input?
if( array_key_exists( "name", $_GET ) && $_GET[ 'name' ] != NULL ) {
    // Get input
    $name = htmlspecialchars( $_GET[ 'name' ] );

    // Feedback for end user
    echo "<pre>Hello ${name}</pre>";
}
?>
```

On avove code user supplied data will be parsed by a function `htmlspecialchars()`. function will encode user input and that encoded data will be passed over server response. 


#### JS

```js
var http = require("http");
var url = require("url");
http.createServer(function(req, res){
  var parsedUrl = url.parse(req.url, true);
  res.writeHead(200, {"Content-Type":"text/html"});
  res.write("Unsanitized input: " + parsedUrl.query.xss);
  res.end();
}).listen(6666);
```

on above code of JS server is listening on port **6666**  and it will result data sent via param as shown below 

```js
http://127.0.0.1:6666/?xss=<XSS>
```
```js
{ href: '/?xss=<XSS>'
, search: '?xss=<XSS>'
, query: { xss: '<XSS>' }
, pathname: '/status'
}
```

#### ASPX

Aspx uses C# for backend. below i took exaple from webgoat.net and there `LoadCity` is function which is vulnerable to XSS
`city` is variable defined passed by city parameter through http get method.

```csharp
 protected void Page_Load(object sender, EventArgs e)
        {
            if (Request["city"] != null)
                LoadCity(Request["city"]);//calls function loadCity with userinput as argument
        }

		void LoadCity (String city)
		{
            DataSet ds = du.GetOffice(city);
            lblOutput.Text = "Here are the details for our " + city + " Office"; //prints user input contaticating with other string.
            dtlView.DataSource = ds.Tables[0];
            dtlView.DataBind();
		}
```

```csharp
#fixed version
void FixedLoadCity (String city)
        {
            DataSet ds = du.GetOffice(city);
            lblOutput.Text = "Here are the details for our " + Server.HtmlEncode(city) + " Office"; //encodes user input
            dtlView.DataSource = ds.Tables[0];
            dtlView.DataBind();
        }
	}
}
```

#### JAVA

Java might not be the easy one to find vunerabilities due to its complax formats and procedure.
below code is vulnerable to XSS as it gets userinpit from `@RequestParam` `field1` and writes same data in response.
```java
public AttackResult completed(@RequestParam Integer QTY1, @RequestParam Integer QTY2, @RequestParam Integer QTY3, @RequestParam Integer QTY4, @RequestParam String field1, @RequestParam String field2) {
    if (field2.toLowerCase().matches(".*<script>.*(console\\.log\\(.*\\)|alert\\(.*\\));?<\\/script>.*"))
      return failed(this).feedback("xss-reflected-5a-failed-wrong-field").build(); 
    double totalSale = QTY1.intValue() * 69.99D + QTY2.intValue() * 27.99D + QTY3.intValue() * 1599.99D + QTY4.intValue() * 299.99D;
    this.userSessionData.setValue("xss-reflected1-complete", "false");
    StringBuffer cart = new StringBuffer();
    cart.append("Thank you for shopping at WebGoat. <br />You're support is appreciated<hr />");
    cart.append("<p>We have charged credit card:" + field1 + "<br />"); //contatination of field1
    cart.append("                             ------------------- <br />");
    cart.append("                               $" + totalSale);
    if (this.userSessionData.getValue("xss-reflected1-complete") == null)
      this.userSessionData.setValue("xss-reflected1-complete", "false"); 
    if (field1.toLowerCase().matches("<script>.*(console\\.log\\(.*\\)|alert\\(.*\\))<\\/script>")) {
      this.userSessionData.setValue("xss-reflected-5a-complete", "true");
      if (field1.toLowerCase().contains("console.log"))
        return success(this).feedback("xss-reflected-5a-success-console").output(cart.toString()).build(); 
      return success(this).feedback("xss-reflected-5a-success-alert").output(cart.toString()).build();
    } 
    this.userSessionData.setValue("xss-reflected1-complete", "false");
    return success(this)
      .feedback("xss-reflected-5a-failure")
      .output(cart.toString())
      .build();
  }
}
```


### Stored XSS

Stored XSS is anonther way to exploit XSS vulnerability in which our payload gets stored in DB and returned when loaded by user or victim. in simple example facebook has comment feature where users can comment. As comments will be saved in DB for later view and persistance. To find ALL types of XSS its not easy to directly find it by reading source code, A proper greybox testing is required for the same.


