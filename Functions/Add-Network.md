![Logo](https://github.com/I-Am-Jakoby/hak5-submissions/blob/main/Assets/logo-170-px.png?raw=true)

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#Description">Description</a></li>
    <li><a href="#The-Function">The Function</a></li>
    <li><a href="#Examples">Examples</a></li>
    <li><a href="#Contact">Contact</a></li>
    <li><a href="#Acknowledgments">Acknowledgments</a></li>
  </ol>
</details>

# Add-NetWork

## Description

This function will add a network profile to your targets PC  

## The Function

### [Add-NetWork] 

This function will accept 3 parameters, 2 of which are mandatory 

You always have to provide the $SSID to give your network a name 

The $Security parameter is also mandatory only takes the value of "t"(true) or "f"(false) 

This will tell the function whether or not you need a wifi password for your network 

If a wifi password is deemed necessary you provide it using the $PW variable 

Set-up a new network profile on your targets PC using the following syntax: 

```
For a network profile using a Password use:

Add-NetWork -SSID wifi-name -security t -PW wifi-password

For a network profile NOT using a Password use:

Add-NetWork -SSID wifi-name -security f

```


```
function Add-NetWork {

[CmdletBinding()]
param (	
[Parameter (Mandatory = $True)]
[string]$SSID,

[Parameter (Mandatory = $True)]
[string]$Security,

[Parameter (Mandatory = $False)]
[string]$PW

)

# -------------------------------------------------------------------------------------------------

$sec = switch ( $Security )
{
    "t"  { 
"
        <security>
            <authEncryption>
                <authentication>WPA2PSK</authentication>
                <encryption>AES</encryption>
                <useOneX>false</useOneX>
            </authEncryption>
            <sharedKey>
                <keyType>passPhrase</keyType>
                <protected>false</protected>
                <keyMaterial>$PW</keyMaterial>
            </sharedKey>
        </security>
"
}
    "f" { 

"
        <security>
            <authEncryption>
                <authentication>open</authentication>
                <encryption>none</encryption>
                <useOneX>false</useOneX>
            </authEncryption>
        </security>
" 

}
}

# -------------------------------------------------------------------------------------------------

$profilefile="ACprofile.xml"
$SSIDHEX=($SSID.ToCharArray() |foreach-object {'{0:X}' -f ([int]$_)}) -join''
$xmlfile="<?xml version=""1.0""?>
<WLANProfile xmlns=""http://www.microsoft.com/networking/WLAN/profile/v1"">
    <name>$SSID</name>
    <SSIDConfig>
        <SSID>
            <hex>$SSIDHEX</hex>
            <name>$SSID</name>
        </SSID>
    </SSIDConfig>
    <connectionType>ESS</connectionType>
    <connectionMode>auto</connectionMode>
    <MSM>
$sec
    </MSM>
</WLANProfile>
"

$XMLFILE > ($profilefile)
netsh wlan add profile filename="$($profilefile)"
}

```

<p align="right">(<a href="#top">back to top</a>)</p>


## Examples 

Listed below are payloads that have used one of these functions:

[PineApple](https://github.com/I-Am-Jakoby/hak5-submissions/tree/main/OMG/Payloads/OMG-PineApple)


<p align="right">(<a href="#top">back to top</a>)</p>

<!-- CONTACT -->
## Contact

<div><h2>I am Jakoby</h2></div>
  <p><br/>

  <img src="https://media.giphy.com/media/VgCDAzcKvsR6OM0uWg/giphy.gif" width="50"> 

  <a href="https://github.com/I-Am-Jakoby/">
    <img src="https://img.shields.io/badge/GitHub-I--Am--Jakoby-blue">
  </a>

  <a href="https://www.instagram.com/i_am_jakoby/">
    <img src="https://img.shields.io/badge/Instagram-i__am__jakoby-red">
  </a>

  <a href="https://twitter.com/I_Am_Jakoby/">
    <img src="https://img.shields.io/badge/Twitter-I__Am__Jakoby-blue">
  </a>

  <a href="https://www.youtube.com/c/IamJakoby/">
    <img src="https://img.shields.io/badge/YouTube-I_am_Jakoby-red">
  </a>

</p>



<p align="right">(<a href="#top">back to top</a>)</p>

<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* [Hak5](https://hak5.org/)
* [UberGuidoZ](https://github.com/UberGuidoZ)

***

[HOME-PAGE](https://github.com/I-Am-Jakoby/PowerShell-for-Hackers)

<p align="right">(<a href="#top">back to top</a>)</p>
