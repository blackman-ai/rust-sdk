# \FeedbackApi

All URIs are relative to *https://app.useblackman.ai/v1*

Method | HTTP request | Description
------------- | ------------- | -------------
[**submit_feedback**](FeedbackApi.md#submit_feedback) | **POST** /v1/feedback | Submit feedback for a completion response



## submit_feedback

> models::SubmitFeedbackResponse submit_feedback(submit_feedback_request)
Submit feedback for a completion response

### Parameters


Name | Type | Description  | Required | Notes
------------- | ------------- | ------------- | ------------- | -------------
**submit_feedback_request** | [**SubmitFeedbackRequest**](SubmitFeedbackRequest.md) |  | [required] |

### Return type

[**models::SubmitFeedbackResponse**](SubmitFeedbackResponse.md)

### Authorization

[BearerAuth](../README.md#BearerAuth)

### HTTP request headers

- **Content-Type**: application/json
- **Accept**: application/json

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

