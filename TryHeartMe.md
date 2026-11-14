# TryHeartMe Walkthrough

## Tools Used
- Browser Developer Tools
- JWT Decoder/Encoder (jwt.one)

## Step 1: Exploring the Website
- The room provides a shopping website where items can be purchased using credits.
- The objective is to find and purchase a hidden item.

## Step 2: Identifying Authentication Mechanism
- Open Developer Tools.
- Navigate to:
  - Application
  - Storage
  - Cookies
- Locate the cookie:
  - tryheartme_jwt
- The cookie contains a JSON Web Token (JWT) used for authentication and authorization.

## Step 3: Decoding the JWT
- Copy the JWT value.
- Open a JWT decoder/encoder.
- Decode the token to inspect its contents.

## Step 4: Modifying Token Data
- Examined the token payload.
- Modified:
  - credits
  - role
- Increased credits to a higher value.
- Changing only credits did not reveal the hidden item.
- Tested different roles.
- The hidden item became visible when the role was changed to:
  - admin

## Step 5: Updating the Cookie
- Generate the updated JWT.
- Return to:
  - Application
  - Storage
  - Cookies
- Replace the existing value of:
  - tryheartme_jwt
- Refresh the page.

## Step 6: Purchasing the Hidden Item
- The updated credits are reflected on the website.
- A previously hidden product now appears.
- Purchase the hidden item.
- The purchase reveals the flag.

## Flag
- THM{v4l3nt1n3_jwt_c00k13_t4mp3r_4dm1n_sh0p}

## Summary
- Identified a JWT used for authentication and authorization.
- Decoded the token and inspected its contents.
- Modified the credits value.
- Changed the role to admin.
- Updated the JWT stored in the cookie.
- Refreshed the application and gained access to the hidden item.
- Purchased the item and retrieved the flag.
