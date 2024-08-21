```
package com.example.startupcheck;

import java.io.IOException;
import java.net.HttpURLConnection;
import java.net.URL;

public class StartupCheckLibrary {

    // URL to be checked
    private static final String URL_TO_CHECK = "https://your-url-to-check.com";
    private static final int RETRY_COUNT = 3;  // Number of retry attempts
    private static final int TIMEOUT_MS = 5000;  // Timeout in milliseconds for each attempt

    static {
        // Perform the URL check on startup
        performStartupCheck();
    }

    /**
     * Performs a startup check by attempting to hit a specified URL. 
     * The application will fail to start if all attempts fail.
     */
    private static void performStartupCheck() {
        boolean success = false;

        for (int attempt = 1; attempt <= RETRY_COUNT; attempt++) {
            try {
                System.out.println("Attempt " + attempt + " to check URL: " + URL_TO_CHECK);
                success = hitUrl(URL_TO_CHECK);

                if (success) {
                    System.out.println("Successfully connected to URL on attempt " + attempt);
                    break;  // Exit loop on success
                }

            } catch (IOException e) {
                System.err.println("Attempt " + attempt + " failed: " + e.getMessage());
            }

            // Optionally, add a delay between retries
            try {
                Thread.sleep(1000);  // 1 second delay
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();  // Restore interrupted state
            }
        }

        if (!success) {
            // Fail the application startup by throwing an exception
            throw new RuntimeException("Failed to connect to URL after " + RETRY_COUNT + " attempts. Application startup aborted.");
        }
    }

    /**
     * Attempts to hit a specified URL.
     *
     * @param urlString The URL to hit.
     * @return true if the URL was successfully hit (HTTP 2xx), false otherwise.
     * @throws IOException If an I/O error occurs.
     */
    private static boolean hitUrl(String urlString) throws IOException {
        URL url = new URL(urlString);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        connection.setConnectTimeout(TIMEOUT_MS);
        connection.setReadTimeout(TIMEOUT_MS);
        connection.setRequestMethod("GET");

        int responseCode = connection.getResponseCode();
        return (responseCode >= 200 && responseCode < 300);  // Return true if HTTP status is 2xx
    }

    // Other library methods can go here
    public void doSomething() {
        System.out.println("Library functionality.");
    }
}
```



'''
package com.example.startupcheck;

import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;
import java.nio.charset.StandardCharsets;

public class JsonPostRequest {

    public static void main(String[] args) {
        try {
            String urlString = "https://your-url-to-post.com/api";
            String jsonPayload = "{\"key1\":\"value1\", \"key2\":\"value2\"}";

            sendPostRequest(urlString, jsonPayload);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * Sends a POST request with a JSON payload.
     *
     * @param urlString   The URL to which the request is sent.
     * @param jsonPayload The JSON payload to send.
     * @throws Exception If an error occurs during the request.
     */
    public static void sendPostRequest(String urlString, String jsonPayload) throws Exception {
        URL url = new URL(urlString);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();

        // Set request method to POST
        connection.setRequestMethod("POST");

        // Set headers for the request
        connection.setRequestProperty("Content-Type", "application/json; utf-8");
        connection.setRequestProperty("Accept", "application/json");

        // Enable writing to the connection output stream
        connection.setDoOutput(true);

        // Write the JSON payload to the output stream
        try (OutputStream os = connection.getOutputStream()) {
            byte[] input = jsonPayload.getBytes(StandardCharsets.UTF_8);
            os.write(input, 0, input.length);
        }

        // Get the response code
        int responseCode = connection.getResponseCode();
        System.out.println("POST Response Code: " + responseCode);

        // Handle the response (optional)
        if (responseCode == HttpURLConnection.HTTP_OK) {
            System.out.println("POST request succeeded.");
            // Read response from the input stream (not shown here)
        } else {
            System.out.println("POST request failed.");
        }

        // Close the connection
        connection.disconnect();
    }
}
'''
