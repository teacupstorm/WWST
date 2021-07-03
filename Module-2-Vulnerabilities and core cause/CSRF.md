# CSRF
**CSRF** is also a vulnerability which is harmful for users or client side (NO server risk). still this vulnerability can chained with other bug and can be critical. In CSRF victim will forced to send data to a website which is vulnerable to CSRF. data can be anything and victim can be a privileged person who can only send data to a website so attacker can do privileged task through victim. A tasks includes.

- attacker posted harmful content to a server through victim. ==> posting comment functionality is vulnerable to CSRF
- attacker commented something on a post through a victim. ==> Comment functionality was vulnerable to CSRF
- attacker transferd amount to his account through victim account. ==> Amount trasfer functionality was vulnerable to CSRF

> In above all cases victim will be unware with the action or tasks performed by him as everything will be done by just visiting a website or a url. 



```php
 <?php

// Get input
$pass_new  = $_GET[ 'password_new' ];
$pass_conf = $_GET[ 'password_conf' ];

// Do the passwords match?
if( $pass_new == $pass_conf ) {
// They do!
$pass_new = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass_new ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
$pass_new = md5( $pass_new );

// Update the database
$insert = "UPDATE `users` SET password = '$pass_new' WHERE user = '" . dvwaCurrentUser() . "';";
$result = mysqli_query($GLOBALS["___mysqli_ston"],  $insert ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

// Feedback for the user
echo "<pre>Password Changed.</pre>";
}
else {
// Issue with passwords matching
echo "<pre>Passwords did not match.</pre>";
}

((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}
?>

```

