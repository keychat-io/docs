# Message Media Types

## Overview

In the context of messaging, Message Media Types play a crucial role in defining the format and content of messages exchanged between users. They provide a standardized way to categorize and handle diverse message types, ensuring seamless communication and appropriate presentation across different platforms.

## Message Media Type Characteristics

Content Identification: Message Media Types enable the sender to identify the type of information conveyed in the message. This could be plain text, a file (image, video, PDF, etc.), or even a contact card.

Receiver Interpretation: The recipient of the message utilizes the Message Media Type to interpret the content appropriately. This involves parsing the message structure and applying the relevant decoding or rendering mechanisms.

Processing and Presentation: Message Media Types influence how the received message is processed and presented within the messaging application. For instance, text messages may be displayed in a chat window, while image files may be opened in an image viewer.

## Message Media Types in Keychat

Keychat implements a specific set of Message Media Types to facilitate the exchange of various message formats. Each type is associated with a unique identifier and a corresponding data structure for representing the message content.

Types of Message Media Types in Keychat

- text: This type represents plain text messages, the most common form of communication in chat applications.

- cashu: ecash red pocket. 1. Content starts with "cashu", 2. parsed into a `CashuInfoModel`

- file: image video file, txt,zip. 1. Content starts with "https://", 2. and the URL query contains a "kctype" value, 3. parsed into a `MsgFileInfo`

- contact: This type identifies a contact card, containing the public key, name, and avatar of a Keychat user. The content can be parsed into `QRUserModel`

## Data Structures for Message Media Types

Keychat defines specific data structures for each Message Media Type to encapsulate the relevant message content. These structures provide a standardized way to represent and process the diverse types of messages exchanged within the application.

### text

```dart
class KeychatMessage {
  MessageType c;
  int type;
  String? msg;
  String? name;
}
```

### Ecash cashu

```dart
class CashuInfoModel {
late String mint;
late String token;
late int amount;
late TransactionStatus status;
String? id;
}
```

### Image / Video / PDF / TXT / OtherFile

```dart
class MsgFileInfo {
  String? localPath;
  String? url;
  FileStatus status = FileStatus.init;
  String? type;
  String? suffix;
  int size = 0;
  String? iv;
  String? key;
}
```

### Contact

```dart
contact
class QRUserModel {
  late String pubkey;
  String? relay;
  String? name;
  String? avatar;
}
```
