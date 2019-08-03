

### What is WhatsApp?
According to [the company](http://www.whatsapp.com/):

> “WhatsApp Messenger is a cross-platform mobile messenger that replaces SMS and works through the existing internet data plan of your device. WhatsApp is available for iPhone, BlackBerry, Android, Windows Phone, Nokia Symbian60 & S40 phones. Because WhatsApp Messenger uses the same internet data plan that you use for email and web browsing, there is no cost to message and stay in touch with your friends.”

Jan. 2015: 30 billion messages per day, ~700 million users.

# License

As of November 1, 2015 Chat API is licensed under the GPLv3+: http://www.gnu.org/licenses/gpl-3.0.html.

# Terms and conditions

- You will NOT use this API for marketing purposes (spam, massive sending...).
- We do NOT give support to anyone that wants this API to send massive messages or similar.
- We reserve the right to block any user of this repository that does not meet these conditions.

## Legal

This code is in no way affiliated with, authorized, maintained, sponsored or endorsed by WhatsApp or any of its affiliates or subsidiaries. This is an independent and unofficial API. Use at your own risk.


## Cryptography Notice

This distribution includes cryptographic software. The country in which you currently reside may have restrictions on the import, possession, use, and/or re-export to another country, of encryption software. BEFORE using any encryption software, please check your country's laws, regulations and policies concerning the import, possession, or use, and re-export of encryption software, to see if this is permitted. See http://www.wassenaar.org/ for more information.

The U.S. Government Department of Commerce, Bureau of Industry and Security (BIS), has classified this software as Export Commodity Control Number (ECCN) 5D002.C.1, which includes information security software using or performing cryptographic functions with asymmetric algorithms. The form and manner of this distribution makes it eligible for export under the License Exception ENC Technology Software Unrestricted (TSU) exception (see the BIS Export Administration Regulations, Section 740.13) for both object code and source code.

# Chat API [![Latest Stable Version](https://poser.pugx.org/whatsapp/chat-api/v/stable)](https://packagist.org/packages/whatsapp/chat-api) [![Total Downloads](https://poser.pugx.org/whatsapp/chat-api/downloads)](https://packagist.org/packages/whatsapp/chat-api) [![License](https://poser.pugx.org/whatsapp/chat-api/license)](https://packagist.org/packages/whatsapp/chat-api)
###############################################################################################################################################################
WhatsAPI Documentation
mgp25 edited this page on Apr 11, 2016 · 65 revisions
Getting Started
Constructor
$username = '';    // Your number with country code, ie: 34123456789
$nickname = '';    // Your nickname, it will appear in push notifications
$debug    = true;  // Shows debug log, this is set to false if not specified
$log      = true;  // Enables log file, this is set to false if not specified

// Create a instance of WhatsProt class.
$w = new WhatsProt($username, $nickname, $debug, $log);
Number registration
$username = "";
$debug = true;

// Create a instance of Registration class.
$r = new Registration($username, $debug);

$r->codeRequest('sms'); // could be 'voice' too
//$r->codeRequest('voice');
You can use this to be more accurate, select the name of your carrier from here

$r->codeRequest($method, $carrier);
Username: Phone number with country code but without '+' or '00', ie: 34123456789

Nickname: The name is going to appear in notifications.

Debug: You can see nodes and more information if you set it to true, if not, set it to false.

When requesting the code, you can do it via SMS or voice call, in both cases you will receive a code like 123-456, that we will use for register the number.

Example of output once requested code:

Array
(
    [cc] => 34
    [in] => *********
    [lg] => es
    [lc] => ES
    [id] => ??i??c?`??
<@sv?
    [token] => 8FzgqwxCb25FH1DmQjqCAN43TdE=
    [mistyped] => 6
    [network_radio_type] => 1
    [simnum] => 1
    [s] => 
    [copiedrc] => 1
    [hasinrc] => 1
    [rcmatch] => 1
    [pid] => 1061
    [rchash] => 1f8b57c1a32b80ae951dedf2db771c9b493f198910ecec1eacc951376d2488eb
    [anhash] => bfaec128347e497017aa9002598a9906
    [extexist] => 1
    [extstate] => 1
    [mcc] => 214
    [mnc] => 000
    [sim_mcc] => 214
    [sim_mnc] => 000
    [method] => sms
)
stdClass Object
(
    [status] => sent
    [length] => 6
    [method] => sms
    [retry_after] => 65
    [sms_wait] => 65
    [voice_wait] => 65
)
Sometimes the message is 'sent' but never arrives, this is because of the mobile networks, you can check the F.A.Q. for more information about this. If you want to request code again with the same number, you will have to wait the retry_after, in this case 10805 seconds.

At this step, we should receive out code, we can register it using this:

If you received the code like this 123-456 you should register like this '123456'

$code = '123456';

$r->codeRegister($code);
If everything went right, this should be the output:

  [status] => ok
  [login] => 34123456789
  [pw] => 0lvOVwZUbvLSxXRk5uYRs3d1E=
  [type] => existing
  [expiration] => 1443256747
  [kind] => free
  [price] => EUR€0.99
  [cost] => 0.89
  [currency] => EUR
  [price_expiration] => 1414897682
The 'pw' is the password and the username is the 'login'. Now you can login to WhatsApp using Chat API!

There is another function used for resetting the password or retrieve a new one.

$r->checkCredentials();
The output will be the same as above. Remember that this will only work if you have the identity you used to register the number saved.

You can use a cli-tool registerTool.php in the /examples folder. You can find it here.

Different errors:

Blocked Your number is blocked.

too_recent Too frequent attempts to request the code.

too_many_guesses Too many wrong code guesses.

too_many Too many attempts. Try again tomorrow.

old_version Protocol version outdated.

stale Registration code expired. You need to request a new one.

missing

If reply param is undefined. Registration code expired. You need to request a new one.
Else. Missing request param
mismatch Invalid registration code entered or different identity used. Please try again.

bad_param Bad parameters passed to code request

no_routes No cell routes for your number caused by your operator

You can also use WART, you can download from here: https://github.com/mgp25/WART

Or you can use this online tool: http://watools.es/pwd.html

Connect and login to WhatsApp
$username = "";
$nickname = "";
$password = ""; // The one we got registering the number
$debug = true;

// Create a instance of WhastPort.
$w = new WhatsProt($username, $nickname, $debug);

$w->connect(); // Connect to WhatsApp network
$w->loginWithPassword($password); // logging in with the password we got!
Using events
You have a few options when using events. The simplest way is the following:

Open up MyEvents.php and uncomment any events you would like to listen for. You should create a public function for each event you want to listen to. See the MyEvents.php for examples.

require '/events/MyEvents.php';

$w = new WhatsProt($username, $nickname, $debug);
$events = new MyEvents($w);
$events->setEventsToListenFor($events->activeEvents); //You can also pass in your own array with a list of events to listen too instead.

//Now continue with your script.
$w->connect();
$w->loginWithPassword($password);
##Legacy Events

We recommend you using the new events system, but you can still use your old code:

function onMessage($mynumber, $from, $id, $type, $time, $name, $body)
{
    echo "Message from $name:\n$body\n\n";
}

$w = new WhatsProt($username, $identity, $nickname, $debug);
$events = new MyEvents($w);
$w->eventManager()->bind("onGetMessage", "onMessage");
This event, when you receive any message it will print 'Message from' and the name of the user who has sent you the message.

Sending messages
Sending text messages:

$target = '34666554433'; // The number of the person you are sending the message
$message = 'Hi! :) this is a test message';

$w->sendMessage($target , $message);
Sending media messages (images, audio, videos):

$target = "34123456789";
$pathToVideo = ""; // This could be url or path to video.
$w->sendMessageVideo($target, $pathToVideo);
$w->pollMessage();

// If you have already the video size and hash, you can send it without re-uploading it to whatsapp servers
$w->sendMessageVideo($target, $filepath, false, $fsize, $fhash, $caption);
$w->pollMessage();
You can add also a caption to videos or images:

$caption = "Look at this awesome image!";
$target = "34123456789";
$pathToVideo = ""; // This could be url or path to image.
$w->sendMessageImage($to, $filepath, false, $fsize, $fhash, $caption);
$w->pollMessage();
And if you want send audio:

$w->sendMessageAudio($target, $filepath, false, $fsize, $fhash);
$w->pollMessage();
You can also send voice messages:

$w->sendMessageAudio($target, $filepath, false, $fsize, $fhash, true);
$w->pollMessage();
Or even documents (only pdf files at the moment):

$w->sendMessageDocument($to, $pdfFile);
Sending messages to a group:

Where $gId is the group id.

$w->sendMessage($gId, $msg);
Sending broadcast message:

$targets = array("34123456789", "34987654321");
$message = "This is broadcast message";
$w->sendBroadcastMessage($targets, $message);
Receiving messages
$w->pollMessage();
You can use an event like onGetMessage, to print the received messages, :

edit events/MyEvents.php and add:

public function onGetMessage( $mynumber, $from, $id, $type, $time, $name, $body )
{
    echo "Message from $name:\n$body\n\n";
}
Remember to uncomment the onGetMessage event at the top of the MyEvent.php file:

Requesting 'Last seen' and profile picture
Requesting 'Last seen':

edit events/MyEvents.php and add:

public function onPresenceUnavailable( $mynumber, $from, $last )
{
  echo "Last seen of $id: ".gmdate("H:i:s", $last);
}
Remember to uncomment the onPresenceUnavailable event at the top of the MyEvent.php file

Now in your own script:

$w->sendPresenceSubscription($to)
Requesting profile picture:

edit events/MyEvents.php and add:

public function onGetProfilePicture( $mynumber, $from, $type, $data )
{
    if ($type == "preview") {
        $filename = "preview_" . $from . ".jpg";
    } else {
        $filename = $from . ".jpg";
    }
    $filename = "ANY FOLDER"."/" . $filename;

    $fp = @fopen($filename, "w");
    if ($fp) {
        fwrite($fp, $data);
        fclose($fp);

    }

    echo '<a href="'.$filename.'"><center><img src="'.$filename.'" height="250" width="250"></center></a><br><br>';

}
Remember to uncomment the onGetProfilePicture event at the top of the MyEvent.php file:

Now in your own script:

$w->sendGetProfilePicture($target, true);
When to get contact profile picture (call sendGetProfilePicture)?

after a number is synced for the first time and exists in WhatsApp
receiving a 'number updated' notification (https://github.com/WHAnonymous/Chat-API/wiki/WhatsAPI-Documentation#notification-nodes)
receiving a 'number added' notification
receiving a 'profile picture changed' notification (onProfilePictureChanged)
Presence subscription/unsubscription
It is used to subscribe every contact in the first login. Unsubscribe when the user has removed account (you will receive a notification) or you remove that contact.

$w->sendPresenceSubscription($to)
$w->sendPresenceUnsubscription($to)
When you subscribe for presence for a given user, you will be instantly notified when the user gets online or offline.

There is no limit of how many users you can track with presence subscription.

When to subscribe for presence (call sendPresenceSubscription)?

One of these:

You can subscribe after a number is synced for the first time and exists in WhatsApp (this covers the situation when you log in for the first time and subscribe to numbers in your existing contact list)
You can subscribe before starting a conversation with a number (number must be synced and existing in WA)
Also always when:

receiving a 'number updated' notification (https://github.com/WHAnonymous/Chat-API/wiki/WhatsAPI-Documentation#notification-nodes), after unsubscribing from presence first
receiving a 'number added' notification
Rule 1: The number should be already synced and existing in WhatsApp before subscribing Rule 2: You must subscribe before starting a conversation with a number Rule 3: Do not subscribe for presence before unsubscribing first

When to unsubscribe from presence (call sendPresenceUnsubscription)?

when removing number from your contact list
when receiving a 'number removed' notification (https://github.com/WHAnonymous/Chat-API/wiki/WhatsAPI-Documentation#notification-nodes)
when receiving a 'number updated' notification (https://github.com/WHAnonymous/Chat-API/wiki/WhatsAPI-Documentation#notification-nodes)
Rule 1: The number should have been synced already Rule 2: Do not unsubscribe for presence before subscribing first

Creating a group
$participants = array("34123456789", "34987654321");
$w->sendGroupsChatCreate("My new Group chat", $participants);
Leaving/Deleting a group
$groupId;
$w->sendGroupsLeave($groupId);
Managing a group
Make someone admin of the group (you must be the owner of the group):

$w->sendPromoteParticipants($gId, $participants);
You can also demote an user, make an admin a normal user again.

$w->sendDemoteParticipants($gId, $participants);
Migrate your number to a new one
You want to migrate from number A to new number B.

Register your new number B

Log in with B

Call sendChangeNumber function

$number = A; // Where A is your old number
$identity = identity of A; // The identity of your old number A

$w->sendChangeNumber($number, $identity);
WhatsApp will now delete your account with A and transfer all the data to your new number B
iqNode:

<iq xmlns="urn:xmpp:whatsapp:account" id="change_number" type="get" to="c.us">
  <modify>
    <username>34****</username>
    <password>identity bytes</password>
  </modify>
</iq>
Result:

<iq from="s.whatsapp.net" id="change_number" type="result">
  <modify>
    <account kind="free" status="active" creation="1419789002" expiration="1452884886"/>
  </modify>
</iq>
Sending vCards
Include vCard.php:

require_once('vCard.php');
Create the vCard:

$v = new vCard();

$v->set('data', array(
	'first_name' => 'John',
	'last_name' => 'Doe',
	'cell_tel' => '9611111111',
));
Send:

$w->sendVcard($target, 'John Doe', $v->show());
Saving messages in db
Only add this line in your script after initialicing WhatsProt class.

$w = new WhatsProt($username, $nickname, $debug);
$w->setMessageStore(new SqliteMessageStore($username));
Messages will be saved in a Sqlite database called msgstore-{your number}.db

List of all events
	onAccountExpired($mynumber, $kind, $status, $creation, $expiration)
	onCallReceived($mynumber, $from, $id, $notify, $time, $callId)
	onClose($mynumber, $error)
	onCodeRegister($mynumber, $login, $password, $type, $expiration, $kind, $price, $cost, $currency, $price_expiration)
	onCodeRegisterFailed($mynumber, $status, $reason, $retry_after)
	onCodeRequest($mynumber, $method, $length)
	onCodeRequestFailed($mynumber, $method, $reason, $param)
	onCodeRequestFailedTooRecent($mynumber, $method, $reason, $retry_after)
	onCodeRequestFailedTooManyGuesses($mynumber, $method, $reason, $retry_after)
	onConnect($mynumber, $socket)
	onConnectError($mynumber, $socket)
	onCredentialsBad($mynumber, $status, $reason)
	onCredentialsGood($mynumber, $login, $password, $type, $expiration, $kind, $price, $cost, $currency, $price_expiration)
	onDisconnect($mynumber, $socket)
	onDissectPhone($mynumber, $phonecountry, $phonecc, $phone, $phonemcc, $phoneISO3166, $phoneISO639, $phonemnc)
	onDissectPhoneFailed($mynumber)
	onGetAudio($mynumber, $from, $id, $type, $time, $name, $size, $url, $file, $mimeType, $fileHash, $duration, $acodec)
	onGetBroadcastLists($mynumber, $broadcastLists)
	onGetDocument($mynumber, $from, $id, $type, $time, $name, $documentData)
	onGetError($mynumber, $from, $id, $data, $errorType = null)
	onGetExtendAccount($mynumber, $kind, $status, $creation, $expiration)
	onGetFeature($mynumber, $from, $encrypt)
	onGetGroupAudio($mynumber, $from_group_jid, $from_user_jid, $id, $type, $time, $name, $size, $url, $file, $mimeType, $fileHash, $duration, $acodec)
	onGetGroupImage($mynumber, $from_group_jid, $from_user_jid, $id, $type, $time, $name, $size, $url, $file, $mimeType, $fileHash, $width, $height, $preview, $caption)
	onGetGroupLocation($mynumber, $from_group_jid, $from_user_jid, $id, $type, $time, $name, $author, $longitude, $latitude, $url, $preview)
	onGetGroupMessage($mynumber, $from_group_jid, $from_user_jid, $id, $type, $time, $name, $body)
	onGetGroups($mynumber, $groupList)
	onGetGroupV2Info($mynumber, $group_id, $creator, $creation, $subject, $participants, $admins, $fromGetGroup)
	onGetGroupsSubject($mynumber, $group_jid, $time, $author, $name, $subject)
	onGetGroupVideo($mynumber, $from_group_jid, $from_user_jid, $id, $type, $time, $name, $url, $file, $size, $mimeType, $fileHash, $duration, $vcodec, $acodec, $preview, $caption)
	onGetGroupvCard($mynumber, $from_group_jid, $from_user_jid, $id, $type, $time, $name, $vcardname, $vcard)
	onGetImage($mynumber, $from, $id, $type, $time, $name, $size, $url, $file, $mimeType, $fileHash, $width, $height, $preview, $caption)
	onGetLocation($mynumber, $from, $id, $type, $time, $name, $author, $longitude, $latitude, $url, $preview)
	onGetMessage($mynumber, $from, $id, $type, $time, $name, $body)
	onGetNormalizedJid($mynumber, $data)
	onGetPrivacyBlockedList($mynumber, $data)
	onGetProfilePicture($mynumber, $from, $type, $data)
	onGetReceipt($from, $id, $offline, $retry)
	onGetServerProperties($mynumber, $version, $props)
	onGetStatus($mynumber, $from, $requested, $id, $time, $data)
	onGetSyncResult($result)
	onGetVideo($mynumber, $from, $id, $type, $time, $name, $url, $file, $size, $mimeType, $fileHash, $duration, $vcodec, $acodec, $preview, $caption)
	onGetvCard($mynumber, $from, $id, $type, $time, $name, $vcardname, $vcard)
	onGroupCreate($mynumber, $groupId)
	onGroupisCreated($mynumber, $creator, $gid, $subject, $admin, $creation, $members = [])
	onGroupsChatCreate($mynumber, $gid)
	onGroupsChatEnd($mynumber, $gid)
	onGroupsParticipantsAdd($mynumber, $groupId, $jid)
	onGroupsParticipantChangedNumber($mynumber, $groupId, $time, $oldNumber, $notify, $newNumber)
	onGroupsParticipantsPromote($myNumber, $groupJID, $time, $issuerJID, $issuerName, $promotedJIDs = [])
	onGroupsParticipantsRemove($mynumber, $groupId, $jid)
	onLoginFailed($mynumber, $data)
	onLoginSuccess($mynumber, $kind, $status, $creation, $expiration)
	onMediaMessageSent($mynumber, $to, $id, $filetype, $url, $filename, $filesize, $filehash, $caption, $icon)
	onMediaUploadFailed($mynumber, $id, $node, $messageNode, $statusMessage)
	onMessageComposing($mynumber, $from, $id, $type, $time)
	onMessagePaused($mynumber, $from, $id, $type, $time)
	onGroupMessageComposing($mynumber, $from_group_jid, $from_user_jid, $id, $type, $time)
	onGroupMessagePaused($mynumber, $from_group_jid, $from_user_jid, $id, $type, $time)
	onMessageReceivedClient($mynumber, $from, $id, $type, $time, $participant)
	onMessageReceivedServer($mynumber, $from, $id, $type, $time)
	onNumberWasAdded($mynumber, $jid)
	onNumberWasRemoved($mynumber, $jid)
	onNumberWasUpdated($mynumber, $jid)
	onPaidAccount($mynumber, $author, $kind, $status, $creation, $expiration)
	onPaymentRecieved($mynumber, $kind, $status, $creation, $expiration)
	onPing($mynumber, $id)
	onPresenceAvailable($mynumber, $from)
	onPresenceUnavailable($mynumber, $from, $last)
	onProfilePictureChanged($mynumber, $from, $id, $time)
	onProfilePictureDeleted($mynumber, $from, $id, $time)
	onSendMessage($mynumber, $target, $messageId, $node)
	onSendMessageReceived($mynumber, $id, $from, $type)
	onSendPong($mynumber, $msgid)
	onSendPresence($mynumber, $type, $name)
	onSendStatusUpdate($mynumber, $txt)
	onStreamError($data)
	onWebSync($mynumber, $from, $id, $syncData, $code, $name)

List of all functions
    checkCredentials()
    codeRegister($code)
    codeRequest($method = 'sms', $countryCode = null, $langCode = null)
    connect()
    disconnect()
    eventManager()
    getMessages()
    loginWithPassword($password)
    pollMessage($autoReceipt = true)
    sendActiveStatus()
    sendBroadcastAudio($targets, $path, $storeURLmedia = false, $fsize = 0, $fhash = "")
    sendBroadcastImage($targets, $path, $storeURLmedia = false, $fsize = 0, $fhash = "", $caption = "")
    sendGetBroadcastLists()
    sendBroadcastLocation($targets, $long, $lat, $name = null, $url = null)
    sendBroadcastMessage($targets, $message)
    sendBroadcastVideo($targets, $path, $storeURLmedia = false, $fsize = 0, $fhash = "", $caption = "")
    sendClearDirty($categories)
    sendGetGroupV2Info 
    sendGetGroupsInfo($gjid)
    sendGetGroupsOwning()
    sendGetGroupsParticipants($gjid)
    sendPromoteParticipants($gId, $participant)
    sendDemoteParticipants($gId, $participant)
    sendGetNormalizedJid($countryCode, $number)
    sendGetPrivacyBlockedList()
    sendGetProfilePicture($number, $large = false)
    sendGetServerProperties()
    sendGetClientConfig()
    sendGetStatuses($jids)
    sendGroupsChatCreate($subject, $participants = array())
    sendSetGroupSubject($gjid, $subject)
    sendGroupsLeave($gjids)
    sendGroupsParticipantsAdd($groupId, $participants)
    sendGroupsParticipantsRemove($groupId, $participants)
    sendMessage($to, $txt, $id = null)
    sendMessageAudio($to, $filepath, $storeURLmedia = false, $fsize = 0, $fhash = "")
    sendMessageComposing($to)
    sendMessageDocument($to, $pdfFile)
    sendMessagePaused($to)
    sendMessageImage($to, $filepath, $storeURLmedia = false, $fsize = 0, $fhash = "", $caption = "")
    sendMessageRead($to, $id)
    sendMessageVideo($to, $filepath, $storeURLmedia = false, $fsize = 0, $fhash = "", $caption = "")
    sendMessageLocation($to, $long, $lat, $name = null, $url = null)
    sendChatState($to, $state)
    sendNextMessage()
    sendOfflineStatus()
    sendPong($msgid)
    sendAvailableForChat($nickname = null)
    sendPing()
    sendPresence($type = "active")
    sendPresenceSubscription($to)
    sendSetGroupPicture($gjid, $path)
    sendSetPrivacyBlockedList($blockedJids = array())
    sendSetProfilePicture($path)
    sendSetRecoveryToken($token)
    sendStatusUpdate($txt)
    sendVcard($to, $name, $vCard)
    sendBroadcastVcard($targets, $name, $vCard)
Nodes
sendGetServerProperties
Function: sendGetServerProperties()

Event: onGetServerProperties( $mynumber, $version, $props )

Request

tx  <iq id="1431035507-2" type="get" xmlns="w" to="s.whatsapp.net">
tx    <props></props>
tx  </iq>
Result

rx  <iq from="s.whatsapp.net" id="1431035507-2" type="result">
rx    <props version="4">
rx      <prop name="timeout" value="300"></prop>
rx      <prop name="location" value="0"></prop>
rx      <prop name="readreceipts" value="1415214000"></prop>
rx      <prop name="qr" value="0"></prop>
rx      <prop name="groups_v2" value="1"></prop>
rx      <prop name="lists" value="1"></prop>
rx      <prop name="library" value="0"></prop>
rx      <prop name="audio" value="1"></prop>
rx      <prop name="checkmarks" value="0"></prop>
rx      <prop name="newmedia" value="0"></prop>
rx      <prop name="video_max_edge" value="960"></prop>
rx      <prop name="image_max_kbytes" value="5120"></prop>
rx      <prop name="image_quality" value="80"></prop>
rx      <prop name="image_max_edge" value="1280"></prop>
rx      <prop name="media" value="16"></prop>
rx      <prop name="broadcast" value="50"></prop>
rx      <prop name="max_subject" value="25"></prop>
rx      <prop name="max_participants" value="101"></prop>
rx      <prop name="max_groups" value="9999"></prop>
rx    </props>
rx  </iq>
sendClientConfig
Function: sendClientConfig()

Request:

tx  <iq id="1431035507-1" type="set" xmlns="urn:xmpp:whatsapp:push" to="s.whatsapp.net">
tx    <config platform="S40" version="2.12.81"></config>
tx  </iq>
Result:

rx  <iq from="s.whatsapp.net" id="1431035507-1" type="result"></iq>
sendGetProfilePicture
Function: sendGetProfilePicture($number, $large = false)

Event: onGetProfilePicture($mynumber, $from, $type, $data)

Request:

tx  <iq id="1431036793-3" type="get" xmlns="w:profile:picture" to="123456789@s.whatsapp.net">
tx    <picture type="preview"></picture>
tx  </iq>
Result:

rx  <iq from="123456789@s.whatsapp.net" id="1431036793-3" type="result">
rx    <picture id="1412155953" type="preview"> 4325 byte data</picture>
rx  </iq>
If the user hasn't a profile picture, you will get this instead.

Result:

rx  <iq from="123456789@s.whatsapp.net" id="1431036888-3" type="error">
rx    <error code="404" text="item-not-found"></error>
rx  </iq>
sendGetNormalizedJid
Function: sendGetNormalizedJid($countryCode, $number)

Request:

tx  <iq id="1413893748-1" xmlns="urn:xmpp:whatsapp:account" type="get" to="s.whatsapp.net">
tx    <normalize>
tx      <cc>34</cc>
tx      <in>123456789</in>
tx    </normalize>
tx  </iq>
Result:

rx  <iq from="s.whatsapp.net" id="1413893748-1" type="result">
rx    <normalize result="34123456789"></normalize>
rx  </iq>
sendPing
Function: sendPing()

Request

tx  <iq id="1413893691-1" xmlns="w:p" type="get" to="s.whatsapp.net">
tx    <ping></ping>
tx  </iq>
Result:

rx  <iq from="s.whatsapp.net" id="1413893691-1" type="result"></iq>
sendGetBroadcastLists
Function: sendGetBroadcastLists()

Request:

tx  <iq id="1413891207-1" xmlns="w:b" type="get" to="s.whatsapp.net">
tx    <lists></lists>
tx  </iq>
Result:

rx  <iq from="s.whatsapp.net" id="1413891207-1" type="result">
rx    <lists>
rx      <list id="123456789@broadcast" name="">
rx        <recipient jid="**********@s.whatsapp.net"></recipient>
rx        <recipient jid="**********@s.whatsapp.net"></recipient>
rx        <recipient jid="**********@s.whatsapp.net"></recipient>
rx        <recipient jid="**********@s.whatsapp.net"></recipient>
rx      </list>
rx      <list id="987654321@broadcast" name="">
rx        <recipient jid="**********@s.whatsapp.net"></recipient>
rx      </list>
rx  </iq>
sendActiveStatus
Function: sendActiveStatus()

Only Tx:

tx  <presence type="active"></presence>
sendMessage
Function: sendMessage($number, $msg)

Event: onMessageReceivedClient($mynumber, $from, $id, $type, $time, $participant)

Event: onMessageReceivedServer($mynumber, $from, $id, $type, $time)

Tx:

tx  <message to="123456789@s.whatsapp.net" type="text" id="1431037793-3" t="1431037793">
tx    <body>Facebook steals your data!</body>
tx  </message>
Rx:

rx  <ack from="123456789@s.whatsapp.net" id="1431037793-3" class="message" t="1431037793"></ack>
System nodes
Features, authing and login

tx  <stream:features>
tx    <readreceipts></readreceipts>
tx    <groups_v2></groups_v2>
tx    <privacy></privacy>
tx    <presence></presence>
tx  </stream:features>

??***00473WhatsApp/2.11.453 Android/4.3 Device/GalaxyS3 MccMnc/515002</auth>

rx  <start from="s.whatsapp.net"></start>

rx  <stream:features></stream:features>

rx  <challenge>??3??5Y??:?Rҍ???N)</challenge>

tx  <response>L?B+?n??U?????ogWn?,???
ê?S?</response>

rx  <success t="1417300474" props="4" kind="free" status="active" creation="1411643804" expiration="1443179804">47&?i9Q?gR???????</success>
Messages received while the number was offline

rx  <ib from="s.whatsapp.net">
rx    <offline count="3"></offline>
rx  </ib>
Presence (Available/Unavailable)

rx  <presence from="***@s.whatsapp.net"></presence>
rx  <presence from="***@s.whatsapp.net" type="unavailable" last="1417300268"></presence>
Presence (Your nickname)

tx  <presence name="Test"></presence>
Notification nodes
Type = contacts

rx  <notification from="xxx" id="12312" type="contacts" t="123">
rx     <remove jid="yyy"></remove>
rx  </notification>
rx  <notification from="xxx" id="12312" type="contacts" t="123">
rx     <add jid="yyy"></add>
rx  </notification>
rx  <notification from="xxx" id="12312" type="contacts" t="123">
rx     <update jid="yyy"></update>
rx  </notification>
add requires you to mark a contact you synced previously as an actual WhatsApp user
remove means that a user is not on WhatsApp anymore, so it becomes just a regular contact
update means that something has changed on the user profile. You should:
reset the presence subscription (unsubscribe and subscribe again)
request the user status
request the user profile image
type = w:gp2

rx  <notification from="xxxx" id="xxx" participant="xxx" type="w:gp2" t="xxx" notify="xxx">
rx      <remove subject="xxx">
rx           <participant jid="xxx"></participant>
rx      </remove>
rx </notification>

rx  <notification from="xxxx" id="xxx" participant="xxx" type="w:gp2" t="xxx" notify="xxx">
rx      <add subject="xxx">
rx           <participant jid="xxx"></participant>
rx      </add>
rx </notification>
add An user was added to a group
remove An user was removes from a group
There are also notification if the group has updates the subject or picture, if the user has been promoted or demoted.

Type = web

rx  <notification from="553499833260@s.whatsapp.net" id="508859621" retry="1" offline="1" type="web" t="1430773333">
rx    <action type="sync">
rx      <sync type="required">@3dpGL6skDNNiUDEzcl3hiQnFhD+11D2odOuwFiHxpB73EHLhCkau2eEl</sync>
rx      <code>EvwDSmI/8wW6Ragp+8rEq6da2zkb9Y2Bg2MGQlrLQoY=</code>
rx      <name>5mwUAgIZF6HLNIR9w+XruQ==</name>
rx    </action>
rx  </notification>
sync webSync
Type = account

rx  <notification from="s.whatsapp.net" id="1636332225" offline="0" type="account" t="1412690302" notify="Server">
rx    <redeem sku="1" delta="365" author="49150000000@s.whatsapp.net">
rx      <account kind="paid" status="active" creation="1409323492" expiration="1503931492"></account>
rx    </redeem>
rx  </notification>
Payment notification, when you have bought WhatsApp subscription one more year.
##WhatsApp Workflow

###Fist Login

$w->connect(); // Connects to WhatsApp

$w->loginWithPassword(); // Login

$w->sendGetPrivacyBlockedList(); // Get our privacy list [Done automatically by the API]

$w->sendGetClientConfig(); // Get client config [Done automatically by the API]

$w->sendGetServerProperties(); // Get server properties [Done automatically by the API]

$w->sendGetGroups(); // Get groups (participating)

$w->sendGetBroadcastLists(); // Get broadcasts lists

$w->sendSync(All contacts); // Sync all contacts

for (All contacts)
{
  $w->sendPresenceSubscription(contact); // subscribe to the user
}

$w->sendGetStatuses(All contacts);

for (All contacts)
{
  $w->sendGetProfilePicture(contact); // preview profile picture of every contact
}
Other logins
$w->connect(); // Connects to WhatsApp

$w->loginWithPassword(); // Login

$w->sendGetPrivacyBlockedList(); // Get our privacy list [Done automatically by the API]

$w->sendGetClientConfig(); // Get client config [Done automatically by the API]
Sending a Message (Contact must be synced)
$w->sendMessageComposing(contact); // Send composing

$w->sendMessagePaused(contact); // Send paused

$w->sendMessage(contact, message); // Send message
Note: If you don't poll enough (pollMessage()), the message won't be sent.


