import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;

public class CreateUserInForgeRock {

    public static void main(String[] args) {
        try {
            // Endpoint URL for creating a new user in ForgeRock IDM
            URL url = new URL("https://your-idm-server.example.com/openidm/managed/user?_action=create");
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();

            // Set the request method and properties
            conn.setRequestMethod("POST");
            conn.setRequestProperty("Content-Type", "application/json");
            conn.setRequestProperty("Authorization", "Bearer YOUR_ACCESS_TOKEN"); // Replace YOUR_ACCESS_TOKEN appropriately
            conn.setDoOutput(true);

            // Prepare the JSON payload for the new user
            String jsonInputString = "{"
                    + "\"userName\": \"newuser\","
                    + "\"givenName\": \"John\","
                    + "\"sn\": \"Doe\","
                    + "\"mail\": \"john.doe@example.com\","
                    + "\"password\": \"password123\""
                    + "}";

            // Write the JSON payload to the request body
            try (OutputStream os = conn.getOutputStream()) {
                byte[] input = jsonInputString.getBytes("utf-8");
                os.write(input, 0, input.length);
            }

            // Read the response from the server
            int responseCode = conn.getResponseCode();
            System.out.println("Response Code: " + responseCode);

            if (responseCode == HttpURLConnection.HTTP_OK) {
                // Handle success
                System.out.println("User created successfully.");
                // Further processing here...
            } else {
                // Handle errors
                System.out.println("Failed to create user.");
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
