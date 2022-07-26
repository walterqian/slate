# Errors

The API uses the following standard error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid. Please check your data.
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- The specified request is for admins only.
404 | Not Found -- The specified request could not be found.
405 | Method Not Allowed -- You tried to access an endpoint with an invalid method.
406 | Not Acceptable -- You requested a format that we don't recognize.
410 | Gone -- The requested endpoint has been removed from our servers.
429 | Too Many Requests -- You've exceeded your rate limit. Please slow down or contact us.
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
