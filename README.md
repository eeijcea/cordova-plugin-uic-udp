cordova-plugin-uic-udp
======================
A Cordova plugin for UDP communications.
Based in the original UDP plugin from applejacko (https://github.com/applejacko/cordova-plugin-uic-udp)
but added new Method (getMessage) for:

Sent UDP request (SSDP Discovery) and wait for Answers.

Only for Android/Java clients!!!

<b>EXAMPLE OF USE (js script at PhoneGap App): </b>

var Devices = [];
var SSDPServer = "";
var SSDPResponse = "";
var SSDPContainer;
var varMSEARCH;

varMSEARCH= "M-SEARCH * HTTP/1.1\r\n";
varMSEARCH += "HOST: 239.255.255.250:1900\r\n";
varMSEARCH += "MAN: \"ssdp:discover\"\r\n";
varMSEARCH += "MX: 8\r\n";
varMSEARCH += "ST: upnp:rootservice\r\n";
varMSEARCH += "USER-AGENT: Android/4.0 UDAP/2.0\r\n";
varMSEARCH += "\r\n";

// 1)  Initialize UDP Stack after Cordova is ready

function onDeviceReady() { 	
 	console.log("Device is ready");		                    
    	connectionStatus = navigator.onLine ? 'online' : 'offline';
    	udptransmit.initialize("239.255.255.250", 1900);
    	return;
 }

// 2)  Submit SSDP Request upon successful UDP Stack initialization

 function UDPTransmitterInitializationSuccess(success) {
		console.log("Sending SSDP Broadcast:\n"+ varMSEARCH);	
		udptransmit.getMessage(varMSEARCH);	
		console.log("Starting Reception....");
	
 }

function UDPTransmitterInitializationError(error) {
   console.log(error);
 }


 //3. Check the success and/or error callbacks to see that each message was transmitted:  
 
function UDPReceptionSuccess(success) {
		console.log("SSDP Broadcast Success:\n");
	   	var allanswers = success;
                //Split Responses (separator = |)
	   	var Rservers = allanswers.split("|");

		// Access to individual fields in the Response (Use :: Like separator)	   	
	   	for (i = 0; i < Rservers.length; i++) {
	   		SSDPContainer = Rservers[i].split("::");
	   		SSDPServer = SSDPContainer[0];
	   		SSDPResponse = SSDPContainer[1];
	   	}	   
	   	
	   console.log(" Number of Rservers found:" + Rservers.length);   

 }
 
 
function UDPReceptionError(error) {
	   console.log(error);
}

