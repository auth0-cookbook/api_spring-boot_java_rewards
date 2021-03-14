# Auth0 Eats Rewards API

You can use this sample to learn how to secure a Spring Boot API server with Auth0. You can then use a client application to practice making secure API calls.

## 1. Get Started

Clone the repository: 

```bash
git clone git@github.com:auth0-cookbook/api_spring-boot_javascript_customers.git
```

Make the project directory your current directory:

```bash
cd api_spring-boot_javascript_customers
```

Install the project dependencies using Gradle:

```bash
./gradlew --refresh-dependencies
```

Open the `application.properties` file in `src/main/resources` and add:

```bash
server.port=6060
```

Finally, run the project by executing the following command:
        

```bash
./gradlew bootRun
```

### API Endpoints

These endpoints require the request to include an access token issued by Auth0 in the authorization header.

#### 🔐 Get customer rewards data

Provides customer rewards data using a customer ID.

```bash
GET /api/rewards/:id
```

##### Response

###### If customer data is not found

```bash
Status: 404 Not Found
```

###### If customer data is found

```bash
Status: 200 OK
```

```json
{
  "id":"9087654321",
  "balance":830,
  "alerts": {
    "text": false,
    "email": true
  }
}
```


## 2. Register an API Server with Auth0

You need an Auth0 account. If you don't have one yet, [sign up for a free Auth0 account](https://auth0.com/signup).

Open the [APIs section of the Auth0 Dashboard](https://manage.auth0.com/#/apis), click the **"Create API"** button.

Fill out the form that comes up:

- **Name:** Auth0 Eats Customers API Sample

- **Identifier:** `https://customers.example.com`

Leave the signing algorithm as `RS256`. It's the best option from a security standpoint.

Once you've added those values, click the **"Create"** button.

Once you create an Auth0 API, a new page loads up, presenting you with your Auth0 API information.

Keep page open to complete the next step.

## 3. Connect the Server with Auth0

Open the `application.properties` file in `src/main/resources` and update it:

```bash
server.port=6060
auth0.audience=
auth0.domain=
spring.security.oauth2.resourceserver.jwt.issuer-uri=https://${auth0.domain}/
```

You'll need to add the values for `AUTH0_AUDIENCE` and `AUTH0_DOMAIN` from your Auth0 API configuration.

Head back to your Auth0 API page, and **follow these steps to get the Auth0 Audience**:

![Get the Auth0 Audience to configure an API](https://cdn.auth0.com/blog/complete-guide-to-user-authentication/get-the-auth0-audience.png)

1. Click on the **"Settings"** tab.

2. Locate the **"Identifier"** field and copy its value.

3. Paste the **"Identifier"** value as the value of `auth0.audience` in `application.properties`.

Now, **follow these steps to get the Auth0 Domain value**:

![Get the Auth0 Domain to configure an API](https://cdn.auth0.com/blog/complete-guide-to-user-authentication/get-the-auth0-domain.png)

1. Click on the **"Test"** tab.
2. Locate the section called **"Asking Auth0 for tokens from my application"**.
3. Click on the **cURL** tab to show a mock `POST` request.
4. Copy your Auth0 domain, which is _part_ of the `--url` parameter value: `tenant-name.region.auth0.com`.
5. Paste the Auth0 domain value as the value of `auth0.domain` in `application.properties`.

**Tips to get the Auth0 Domain**

- The Auth0 Domain is the substring between the protocol, `https://` and the path `/oauth/token`.

- The Auth0 Domain follows this pattern: `tenant-name.region.auth0.com`.
 
- The `region` subdomain (`au`, `us`, or `eu`) is optional. Some Auth0 Domains don't have it.

- **Click on the image above, please, if you have any doubt on how to get the Auth0 Domain value**.

With the `application.properties` configuration values set, you need to restart the local server so that Spring Boot can see these new environment variables. Stop the running process and execute `./gradlew bootRun` once again.

## 4. Test the Server

You need your API server root URL to make requests.

The local server root URL is `http://localhost:6060`.

### Test a protected endpoint

You need an access token to call any of the protected API endpoints.

Try to make the following request:

```bash
curl <SERVER_ROOT_URL>/api/rewards/9087654321
```

You'll get the following response error:

```bash
No authorization token was found
```

To get an access token, head back to your API configuration page in the Auth0 Dashboard.

Click on the **"Test"** tab and locate the **"Sending the token to the API"**.

Click on the **"cURL"** tab.

You should see something like this:

```bash
curl --request GET \
  --url <SERVER_ROOT_URL>/path_to_your_api/ \
  --header 'authorization: Bearer really-long-string'
```

Copy and paste that value in a text editor.

In the value of the `--header` parameter, the value of `really-long-string` is your access token.

Replace the value of the `--url` parameter with your `GET api/rewards/:id` endpoint URL:

```bash
curl --request GET \
  --url <SERVER_ROOT_URL>/api/rewards/9087654321 \
  --header 'authorization: Bearer really-long-string'
```

Copy and paste the updated cURL command into a terminal window and execute it. You should now get a valid response.

You can also use any of the Auth0 Eats client applications to consume this API. The client applications require users to log in, obtaining an access token in the background, before they can call the Auth0 Eats Customers API.

