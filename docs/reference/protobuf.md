# KubeFox Protocol Buffers




<a name="kubefoxprotov1ack"></a>

### Ack







<a name="kubefoxprotov1data"></a>

### Data



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| type | [string](#string) |  |  |
| id | [string](#string) |  |  |
| request_id | [string](#string) |  |  |
| source | [string](#string) |  |  |
| target | [string](#string) |  |  |
| system_tag | [string](#string) |  |  |
| environment | [string](#string) |  |  |
| span | [Span](#kubefoxprotov1span) |  |  |
| labels | [Data.LabelsEntry](#kubefoxprotov1datalabelsentry) | repeated |  |
| claims | [Data.ClaimsEntry](#kubefoxprotov1dataclaimsentry) | repeated |  |
| args | [google.protobuf.Struct](#googleprotobufstruct) |  |  |
| values | [google.protobuf.Struct](#googleprotobufstruct) |  |  |
| content_type | [string](#string) |  |  |
| content | [bytes](#bytes) |  |  |






<a name="kubefoxprotov1dataclaimsentry"></a>

### Data.ClaimsEntry



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| key | [string](#string) |  |  |
| value | [string](#string) |  |  |






<a name="kubefoxprotov1datalabelsentry"></a>

### Data.LabelsEntry



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| key | [string](#string) |  |  |
| value | [string](#string) |  |  |






<a name="kubefoxprotov1span"></a>

### Span



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| trace_id | [string](#string) |  |  |
| span_id | [string](#string) |  |  |
| trace_flags | [bytes](#bytes) |  |  |






<a name="kubefoxprotov1subscriberequest"></a>

### SubscribeRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id | [string](#string) |  |  |






<a name="kubefoxprotov1telemetryrequest"></a>

### TelemetryRequest







<a name="kubefoxprotov1telemetryresponse"></a>

### TelemetryResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| healthy | [bool](#bool) |  |  |





 <!-- end messages -->

 <!-- end enums -->

 <!-- end HasExtensions -->


<a name="kubefoxprotov1componentservice"></a>

### ComponentService


| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| InvokeRemoteComponent | [Data](#kubefoxprotov1data) | [Data](#kubefoxprotov1data) |  |
| Subscribe | [SubscribeRequest](#kubefoxprotov1subscriberequest) | [Data](#kubefoxprotov1data) stream |  |
| Unsubscribe | [SubscribeRequest](#kubefoxprotov1subscriberequest) | [Ack](#kubefoxprotov1ack) |  |
| SendResponse | [Data](#kubefoxprotov1data) | [Ack](#kubefoxprotov1ack) |  |

 <!-- end services -->



## Scalar Value Types

| .proto Type | Notes | C++ | Java | Python | Go | C# | PHP | Ruby |
| ----------- | ----- | --- | ---- | ------ | -- | -- | --- | ---- |
| <a name="double" /> double |  | double | double | float | float64 | double | float | Float |
| <a name="float" /> float |  | float | float | float | float32 | float | float | Float |
| <a name="int32" /> int32 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="int64" /> int64 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="uint32" /> uint32 | Uses variable-length encoding. | uint32 | int | int/long | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="uint64" /> uint64 | Uses variable-length encoding. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum or Fixnum (as required) |
| <a name="sint32" /> sint32 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sint64" /> sint64 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="fixed32" /> fixed32 | Always four bytes. More efficient than uint32 if values are often greater than 2^28. | uint32 | int | int | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="fixed64" /> fixed64 | Always eight bytes. More efficient than uint64 if values are often greater than 2^56. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum |
| <a name="sfixed32" /> sfixed32 | Always four bytes. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sfixed64" /> sfixed64 | Always eight bytes. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="bool" /> bool |  | bool | boolean | boolean | bool | bool | boolean | TrueClass/FalseClass |
| <a name="string" /> string | A string must always contain UTF-8 encoded or 7-bit ASCII text. | string | String | str/unicode | string | string | string | String (UTF-8) |
| <a name="bytes" /> bytes | May contain any arbitrary sequence of bytes. | string | ByteString | str | []byte | ByteString | string | String (ASCII-8BIT) |
