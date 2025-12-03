# CompletionRequest

## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**max_tokens** | Option<**i32**> |  | [optional]
**stop** | Option<**Vec<String>**> |  | [optional]
**stream** | Option<**bool**> |  | [optional]
**temperature** | Option<**f32**> |  | [optional]
**top_p** | Option<**f32**> |  | [optional]
**messages** | [**Vec<models::Message>**](Message.md) |  | 
**metadata** | Option<[**serde_json::Value**](.md)> | Optional metadata for tracking, analytics, and conditional processing. Can include session IDs, user context, feature flags, or any custom data. This metadata is logged with the request and can be used for filtering/analysis. | [optional]
**model** | **String** |  | 
**provider** | [**models::Provider**](Provider.md) |  | 

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


