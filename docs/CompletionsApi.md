# \CompletionsApi

All URIs are relative to *https://app.useblackman.ai/v1*

Method | HTTP request | Description
------------- | ------------- | -------------
[**completions**](CompletionsApi.md#completions) | **POST** /v1/completions | 



## completions

> models::CompletionResponse completions(completion_request, x_provider_api_key)


### Parameters


Name | Type | Description  | Required | Notes
------------- | ------------- | ------------- | ------------- | -------------
**completion_request** | [**CompletionRequest**](CompletionRequest.md) |  | [required] |
**x_provider_api_key** | Option<**String**> | Optional provider API key to override stored or system keys |  |

### Return type

[**models::CompletionResponse**](CompletionResponse.md)

### Authorization

[BearerAuth](../README.md#BearerAuth)

### HTTP request headers

- **Content-Type**: application/json
- **Accept**: application/json

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

