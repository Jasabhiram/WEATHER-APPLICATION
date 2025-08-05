import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Scanner;

public class WeatherApp {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        final String apiKey = "0f49e660a4aa4c2f606410083c9c46ba";
        boolean success = false;

        while (!success) {
            System.out.print("Enter city name: ");
            String city = scanner.nextLine().trim();

            String urlString = String.format(
                "https://api.openweathermap.org/data/2.5/weather?q=%s&appid=%s&units=metric",
                city, apiKey);

            try {
                URL url = new URL(urlString);
                HttpURLConnection conn = (HttpURLConnection) url.openConnection();
                conn.setRequestMethod("GET");

                int responseCode = conn.getResponseCode();

                if (responseCode == HttpURLConnection.HTTP_OK) {
                    BufferedReader in = new BufferedReader(
                        new InputStreamReader(conn.getInputStream()));
                    String inputLine;
                    StringBuilder response = new StringBuilder();

                    while ((inputLine = in.readLine()) != null) {
                        response.append(inputLine);
                    }
                    in.close();

                    System.out.println("Weather Data:");
                    System.out.println(response.toString());
                    success = true; // Exit loop after successful fetch
                } else {
                    BufferedReader in = new BufferedReader(
                        new InputStreamReader(conn.getErrorStream()));
                    String inputLine;
                    StringBuilder errorResponse = new StringBuilder();

                    while ((inputLine = in.readLine()) != null) {
                        errorResponse.append(inputLine);
                    }
                    in.close();

                    System.out.println("City not found. Try again.");
                    System.out.println("Error: " + errorResponse.toString());
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
