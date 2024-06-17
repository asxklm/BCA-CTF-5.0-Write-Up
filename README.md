# Writeup-BCACTF5.0

Here's a GitHub README template based on the analysis and write-up of the BCA CTF 5.0 challenges:

---

# BCA CTF 5.0 Write-Up

## Introduction
Welcome to the BCA CTF 5.0 Write-Up! This document details the solutions for the challenges presented in the Capture The Flag (CTF) competition.

## Table of Contents
1. [Inaccessible (binex)](#inaccessible-binex)
2. [Tic-Tac-Toe (webex)](#tic-tac-toe-webex)
3. [XOR (crypto)](#xor-crypto)
4. [NOTflag (crypto)](#notflag-crypto)
5. [Time Skip (crypto)](#time-skip-crypto)
6. [Vinegar Times 3 (crypto)](#vinegar-times-3-crypto)
7. [Chalkboard Gag (forensic)](#chalkboard-gag-forensic)
8. [Sea Scavenger (forensic)](#sea-scavenger-forensic)

## Inaccessible (binex)
### Description
Decompile the binary and call the `win` function to get the flag.

### Solution
```c
void win(void) {
    // Function to print the flag
}
```

### Flag
```
bcactf{W0w_Y0u_m4d3_iT_b810c453a9ac9}
```

## Tic-Tac-Toe (webex)
### Description
Win the tic-tac-toe game using Burp Suite.

### Solution
Intercept WebSocket communication and modify the game state.

### Flag
```
bcactf{7h3_m4st3r_0f_t1ct4ct0e_678d52c8}
```

## XOR (crypto)
### Description
Decrypt XOR encrypted flag using the given key.

### Solution
Used the ASCII key `ClkvKOR8JQA1JB731LeGkU7J4d2khDvrOPI63mM7` to decrypt.

### Flag
```
bcactf{SYMmE7ric_eNcrYP710N_4WD0f229}
```

## NOTflag (crypto)
### Description
Decode the Base64 encrypted flag.

### Solution
Used a Base64 decoder.

### Flag
```
bcactf{7hIs_1s_7h3_fla9}
```

## Time Skip (crypto)
### Description
Decrypt the Scytale cipher text.

### Solution
Used an online Scytale cipher decryption tool.

### Flag
```
bcactf{5c7t4l3_h15t04y_qe829xl1}
```

## Vinegar Times 3 (crypto)
### Description
Decrypt the Vigenere cipher using multiple keys.

### Solution
Sequential decryption with keys `vinegar`, `redwine`, and `balsamic`.

### Flag
```
bcactf{add_to_salad_yummy}
```

## Chalkboard Gag (forensic)
### Description
Find differences in repeated phrases.

### Solution
Custom Python script to extract deviations.

### Flag
```
bcactf{BaRT_W0U1D_B3_PR0uD}
```

## Sea Scavenger (forensic)
### Description
Find the specific ID from HTML comments and access the URL.

### Solution
Used cURL to fetch the URL with the derived ID.

### Flag
```
bcactf{R3gex_WH1z_54dfa9cdba13}
```

## Conclusion
This document provides the detailed solutions for each challenge in BCA CTF 5.0. Each section outlines the challenge, the approach used, and the final flag obtained.

---

Feel free to copy this into your `README.md` file on GitHub.
