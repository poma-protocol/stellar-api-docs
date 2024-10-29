# API Documentation

## Overview
This API documentation outlines the endpoints and functionality of a Stellar-based API server. The server provides various endpoints for interacting with the Stellar network.

---

### General Information

**Base URL:** `http://localhost:3000`

**Headers (for all routes):**
- **Content-Type**: `application/json`

---

## API Endpoints

### 1. `GET /`

- **Description**: Simple health check for the API.
- **Request**: No body is required.
- **Response**:
  - **Status Code**: `200 OK`
  - **Body**: `{ "message": "Hello World!" }`

---

### 2. `POST /send-stellar`

- **Description**: Send Stellar tokens to a receiver.
- **Method**: `POST`
- **Request Body**:
  ```json
  {
    "receiver": "string", // Required. Stellar public key of the receiver.
    "amount": "number" // Required. Amount to send.
  }
  ```
- **Response**:
  - **Status Code**: 
    - `200 OK`: Transaction was successful.
    - `400 Bad Request`: Invalid input (missing fields, invalid address, or invalid amount).
    - `500 Internal Server Error`: Treasury account issues or other server errors.
  - **Body**:
    ```json
    {
      "message": "Transaction Successful",
      "receiver": "string",
      "amount": "number",
      "txn_url": "string" // URL to the transaction on Stellar Explorer.
    }
    ```
  - **Error Example**:
    ```json
    {
      "message": "Error Occurred: {error_message}"
    }
    ```

---

### 3. `POST /check-receiver-trust`

- **Description**: Check if a receiver has a trustline established for a specific asset.
- **Method**: `POST`
- **Request Body**:
  ```json
  {
    "asset_receiver": "string", // Required. Public key of the receiver.
    "asset_code": "string", // Required. Asset code (e.g., "USDC").
    "asset_issuer": "string" // Required. Public key of the asset issuer.
  }
  ```
- **Response**:
  - **Status Code**:
    - `200 OK`: Successfully retrieved trustline status.
    - `400 Bad Request`: Invalid input or account not found on-chain.
  - **Body**:
    ```json
    {
      "trustline": "boolean", // Whether trustline exists.
      "balance": "string", // If trustline exists, current balance of asset.
      "limit": "string", // If trustline exists, maximum limit of asset.
      "remainingLimit": "number" // Remaining limit for the asset.
    }
    ```
  - **Error Example**:
    ```json
    {
      "message": "Error Occurred: {error_message}"
    }
    ```

---

### 4. `POST /check-trust`

- **Description**: Check if the treasury account has a trustline for a specific asset.
- **Method**: `POST`
- **Request Body**:
  ```json
  {
    "asset_code": "string", // Required. Asset code.
    "asset_issuer": "string" // Required. Public key of the asset issuer.
  }
  ```
- **Response**:
  - **Status Code**:
    - `200 OK`: Successfully retrieved treasury trustline status.
    - `400 Bad Request`: Invalid input or account not found.
  - **Body**:
    ```json
    {
      "trustline": "boolean",
      "balance": "string",
      "limit": "string",
      "remainingLimit": "number"
    }
    ```
  - **Error Example**:
    ```json
    {
      "message": "Error Occurred: {error_message}"
    }
    ```

---

### 5. `POST /make-trust`

- **Description**: Create a trustline for the treasury account for a specific asset.
- **Method**: `POST`
- **Request Body**:
  ```json
  {
    "asset_code": "string", // Required. Asset code.
    "asset_issuer": "string" // Required. Public key of the asset issuer.
  }
  ```
- **Response**:
  - **Status Code**:
    - `200 OK`: Trustline successfully created.
    - `400 Bad Request`: Invalid input or account not found.
    - `500 Internal Server Error`: Failed to create trustline.
  - **Body**:
    ```json
    {
      "message": "Trustline Created Successfully",
      "txn_url": "string" // URL to the transaction on Stellar Explorer.
    }
    ```
  - **Error Example**:
    ```json
    {
      "message": "Error Occurred: {error_message}"
    }
    ```

---

### 6. `POST /check-nft-balance`

- **Description**: Check the balance of an NFT asset for the treasury account.
- **Method**: `POST`
- **Request Body**:
  ```json
  {
    "asset_code": "string", // Required. Asset code of the NFT.
    "asset_issuer": "string" // Required. Public key of the asset issuer.
  }
  ```
- **Response**:
  - **Status Code**:
    - `200 OK`: Successfully retrieved NFT balance.
    - `400 Bad Request`: Invalid input or account not found.
  - **Body**:
    ```json
    {
      "balance": "string",
      "limit": "string",
      "remainingLimit": "number"
    }
    ```
  - **Error Example**:
    ```json
    {
      "message": "Error Occurred: {error_message}"
    }
    ```

---

### 7. `POST /claim-nft`

- **Description**: Claim NFT balance for the treasury account.
- **Method**: `POST`
- **Request Body**:
  ```json
  {
    "asset_code": "string", // Required. Asset code of the NFT.
    "asset_issuer": "string", // Required. Public key of the asset issuer.
    "amount": "number" // Required. Amount to claim.
  }
  ```
- **Response**:
  - **Status Code**:
    - `200 OK`: NFT successfully claimed.
    - `400 Bad Request`: No claimable balance found or insufficient amount.
  - **Body**:
    ```json
    {
      "message": "NFTs Claimed Successfully",
      "txn_url": "string" // URL to the transaction on Stellar Explorer.
    }
    ```
  - **Error Example**:
    ```json
    {
      "message": "Error Occurred: {error_message}"
    }
    ```

---

### 8. `POST /send-nft`

- **Description**: Send an NFT from the treasury account to a receiver.
- **Method**: `POST`
- **Request Body**:
  ```json
  {
    "asset_code": "string", // Required. Asset code of the NFT.
    "asset_issuer": "string", // Required. Public key of the asset issuer.
    "asset_receiver": "string" // Required. Public key of the receiver.
  }
  ```
- **Response**:
  - **Status Code**:
    - `200 OK`: NFT successfully sent.
    - `400 Bad Request`: Invalid input or account not found.
    - `500 Internal Server Error`: Error sending NFT.
  - **Body**:
    ```json
    {
      "message": "NFT Sent Successfully",
      "txn_url": "string" // URL to the transaction on Stellar Explorer.
    }
    ```
  - **Error Example**:
    ```json
    {
      "message": "Error Occurred: {error_message}"
    }
    ```
