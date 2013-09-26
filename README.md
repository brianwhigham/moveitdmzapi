moveitdmzapi
============

The basics of how to talk to the IpSwitch MoveIt DMZ managed file transfer server.

# Overview
The perl libraries provided by IpSwitch are no longer maintained, but are still available for download.  Java and .net (I guess) libraries are kept up to date.  [The IpSwitch documentation](https://moveitsupport.ipswitch.com/SUPPORT/miapiwin/online-manual.htm) is helpful, but not directly relavent to those who want to use the RESTful API.  They do not provide a document for the RESTful API.  Furthermore, I'm not sure how often it might change between versions.

I've asked that the old perl libraries be opensourced.  You can ask too [by using this form](http://www.ipswitchft.com/company/contactsupport.aspx) or calling (781) 645-5570 between 8:00 am and 6:00 pm Central U.S. time zone (GMT-6), Monday through Friday. Better yet, submit a support ticket.

The endpoint is at https://acme.com/machine.aspx

You need a minimum of three arguments in the GET string
1. transaction
2. username
3. password

Other arguments are specified with *arg#*, numbered as the method requires.  The transaction is essentially the action or query that you want to issue to the server.  Sometimes the [method names in the MoveIt DMZ API documentation](https://moveitsupport.ipswitch.com/SUPPORT/miapiwin/online-manual.htm) translate to their lcased transaction names.  Sometimes, they don't quite match

# Tools
Interacting primitively with the API is eased by some great tools

* [curl](http://curl.haxx.se/) - for making arbitrary HTTP calls from the command line
* [Chrome REST Console](https://chrome.google.com/webstore/detail/rest-console/cokgbflfommojglbmbpenpphppikmonn?hl=en)
* [XML::XPath](http://search.cpan.org/~msergeant/XML-XPath-1.13/XPath.pm) - for searching the XML responses from the command-line (xpath command included with libxml-xpath-perl Ubuntu package)

# What Happens If The Server Isn't Licensed for API Use?
curl 'https://acme.com/machine.aspx?transaction=folderlist&username=bob&password=seekret'
```xml
<?xml version="1.0"?>
<siLockResponse><ErrorCode>3400</ErrorCode><ErrorDescription>Licensing error: This key does not enable this feature.</ErrorDescription><Payload></Payload></siLockResponse>
```

# Examples
## list folders
curl 'https://files.acme.com/machine.aspx?transaction=folderlist&username=bob&password=seekret'
## make group member an admin
curl 'https://files.acme.com/machine.aspx?transaction=usergroupchangemembership&username=bob&password=seekret&arg01=coolkids&arg02=bob&arg03=2'
## list a user's groups
curl 'https://files.acme.com/machine.aspx?transaction=usergrouplist&username=bob&password=seekret&arg01=bob'

here's an example of xpath usage and output:
curl 'https://files.acme.com/machine.aspx?transaction=usergrouplist&username=bob&password=seekret&arg01=bob'|xpath -e "//Name"

```xml
<Name>georgiaview-930</Name>
<Name>georgiaview-980</Name>
<Name>test-01</Name>
```
