Looking at the consent API logs, I can see a clear networking issue that's causing the error. Let me analyze this in detail:

## Root Cause Analysis for Consent API Error

The error occurs when the system attempts to make a connection to the add consent endpoint at `https://cgsg-in-sit4.bmogc.net:8076/esbapi/party/bmopartyconsent/v1/addbmopartyconsent`.

The specific error is:
```
java.net.ConnectException: Connection refused (Connection refused)
```

This is a fundamental networking error indicating that the client tried to establish a TCP connection to the server, but the server actively refused the connection. This typically happens when:

1. The server application is not running on the specified port (8076)
2. A firewall is blocking the connection
3. The network route to the server is unavailable

## Sequence of Events

1. The API is able to successfully call `getbmoprocessingpurposePost` at port 8106 
2. The API is able to successfully call `getallbmopartyconsentsPost` at port 8086
3. The API fails when trying to call `addbmopartyconsentPost` at port 8076 with "Connection refused"

## Key Details

- Error occurs at timestamp 2025-03-21T13:55:37.736+0000
- Target server: cgsg-in-sit4.bmogc.net (IP: 10.197.152.44)
- Target port: 8076
- HTTP method: POST
- Endpoint: /esbapi/party/bmopartyconsent/v1/addbmopartyconsent

## Recommendations for the Backend Team

1. **Verify Service Availability**:
   - Check if the consent service is running on port 8076 on cgsg-in-sit4.bmogc.net
   - Confirm the service is properly deployed and active

2. **Network Configuration**:
   - Verify firewall rules allow connections to port 8076
   - Check network connectivity between the application server and the consent service

3. **Service Deployment**:
   - Verify that all components of the bmopartyconsent service are properly deployed
   - Ensure that port configurations match between environments

4. **Immediate Action**:
   - Run network diagnostic commands (ping, telnet, netstat) to verify connectivity
   - Check service logs on the target server to see if there are any errors or if the service is running

The critical detail is that other related services on different ports (8106 and 8086) are working correctly, but the specific service on port 8076 is not accepting connections, which strongly suggests a service-specific issue rather than a network-wide problem.

This appears to be a separate issue from the previous authentication problems and relates specifically to the consent management service infrastructure.



Yes, we are now able to see the calls proceeding after the getbmoprocessingpurpose call. Previously, the error occurred immediately after that call. I'm not sure which team is responsible for this, but I remember that last time you reached out to the support team for assistance
