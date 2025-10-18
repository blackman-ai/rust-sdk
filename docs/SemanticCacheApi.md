# \SemanticCacheApi

All URIs are relative to *https://app.useblackman.ai/v1*

Method | HTTP request | Description
------------- | ------------- | -------------
[**get_config**](SemanticCacheApi.md#get_config) | **GET** /v1/semantic-cache/config | Get semantic cache configuration for the current account
[**get_stats**](SemanticCacheApi.md#get_stats) | **GET** /v1/semantic-cache/stats | Get semantic cache statistics including hit rate and savings
[**invalidate_all**](SemanticCacheApi.md#invalidate_all) | **DELETE** /v1/semantic-cache/invalidate | Invalidate all cache entries for the current account
[**update_config**](SemanticCacheApi.md#update_config) | **PUT** /v1/semantic-cache/config | Update semantic cache configuration for the current account



## get_config

> models::SemanticCacheConfig get_config()
Get semantic cache configuration for the current account

### Parameters

This endpoint does not need any parameter.

### Return type

[**models::SemanticCacheConfig**](SemanticCacheConfig.md)

### Authorization

[BearerAuth](../README.md#BearerAuth)

### HTTP request headers

- **Content-Type**: Not defined
- **Accept**: application/json

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)


## get_stats

> models::SemanticCacheStats get_stats()
Get semantic cache statistics including hit rate and savings

### Parameters

This endpoint does not need any parameter.

### Return type

[**models::SemanticCacheStats**](SemanticCacheStats.md)

### Authorization

[BearerAuth](../README.md#BearerAuth)

### HTTP request headers

- **Content-Type**: Not defined
- **Accept**: application/json

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)


## invalidate_all

> models::InvalidateResponse invalidate_all()
Invalidate all cache entries for the current account

### Parameters

This endpoint does not need any parameter.

### Return type

[**models::InvalidateResponse**](InvalidateResponse.md)

### Authorization

[BearerAuth](../README.md#BearerAuth)

### HTTP request headers

- **Content-Type**: Not defined
- **Accept**: application/json

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)


## update_config

> update_config(semantic_cache_config)
Update semantic cache configuration for the current account

### Parameters


Name | Type | Description  | Required | Notes
------------- | ------------- | ------------- | ------------- | -------------
**semantic_cache_config** | [**SemanticCacheConfig**](SemanticCacheConfig.md) |  | [required] |

### Return type

 (empty response body)

### Authorization

[BearerAuth](../README.md#BearerAuth)

### HTTP request headers

- **Content-Type**: application/json
- **Accept**: Not defined

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

