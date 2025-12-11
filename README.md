# MT5 Device Fingerprint Tool

A lightweight and reliable **HWID-based device fingerprint generator** for **MetaTrader 5**, designed to help developers protect paid indicators, EAs, and scripts.  
This tool collects stable terminal and account identifiers, combines them into a normalized machine signature, and produces a **SHA-256 license code** without using any external DLLs.  

Ideal for sellers who need **per-user licensing**, **hardware locking**, and **simple offline verification** inside their MQL5 products.

---

## Features

- Generates a unique **device fingerprint** from stable MT5 properties  
- Produces a **SHA-256 hash** for licensing (no external dependencies)  
- Works fully inside MetaTrader 5 (pure MQL5 implementation)  
- Zero DLL imports — fully compatible with MetaTrader Market rules  
- Easy integration with commercial indicators and EAs  
- Clean, readable, well-structured code  
- Includes raw machine ID + final hashed license code  
- Suitable for offline licensing workflows

---

## Use Case

This tool is commonly used for:

- Selling MT5 indicators that must run only on specific accounts or machines  
- Verifying customers before delivering the main product  
- Reinforcing anti-piracy measures in commercial EAs  
- Binding licenses to client hardware or trading account  
- Allowing manual approval of each client via hashed code

---

## How It Works

1. The user installs the fingerprint indicator.  
2. The indicator collects a set of non-changing identifiers:
   - Account login  
   - Broker server  
   - Terminal company  
   - Terminal paths  
   - Common data path  
3. The concatenated string is hashed using MT5's built-in `CryptEncode` with **CRYPT_HASH_SHA256**.  
4. The user sends the hash to the seller.  
5. The seller embeds this hash inside the final product (indicator/EA).  
6. The main indicator checks the customer’s hardware hash at runtime and stops execution if not authorized.

---

## Example Output

When the indicator runs, it displays something like:

```
Your License Code: 3A9F1E3C77D50D0E25F0A53BD4E21D7DF917B8F9EC57E3E82890F2C542A19F5C
```

This code can then be stored inside your licensed product.

---

## Installation

1. Clone the repository:

```
git clone [https://github.com/BaseMax/mt5-device-fingerprint-tool](https://github.com/BaseMax/mt5-device-fingerprint-tool)
```

2. Copy `mt5-device-fingerprint-tool.mq5` (or the provided file) into:

```
MQL5/Indicators/
```

3. Compile the file in MetaEditor.  
4. Attach the indicator to any chart inside MetaTrader 5.

---

## Code Overview

The core functions include:

- `GetMachineID()`  
  Collects stable system/account identifiers.

- `SHA256_hex_from_string()`  
  Generates a SHA-256 hex hash using MT5's built-in cryptography.

- `CharArrayToHex()`  
  Utility to convert raw bytes to an uppercase hex string.

The generated hash is cryptographically strong and machine-specific.

---

## Licensing Your Own Indicator / EA

Inside your commercial product, implement a check similar to:

```mq5
string AllowedHash = "PUT_CUSTOMER_HASH_HERE";

if (DeviceHash != AllowedHash)
{
    Alert("License error – unauthorized machine.");
    return INIT_FAILED;
}
```

You may also close the chart or stop execution depending on your protection strategy.

---

## Roadmap

* Add optional obfuscation for machine ID
* Add SHA-1 and MD5 fallback options
* Add server-based online activation mode
* Provide a full licensing framework (client + server)
* Add ready-to-use LicensedIndicator.mq5 template

---

## Contributing

Contributions, feature requests, and pull requests are welcome.
If you implement new fingerprint parameters or stronger protection layers, feel free to submit them.

---

## License

Copyright (c) 2025

MIT License

See the [LICENSE](LICENSE) file for details.

---

## Author

**Seyyed Ali Mohammadiyeh (Max Base)**

GitHub: [https://github.com/BaseMax](https://github.com/BaseMax)

---

## Star the Repo

If you find this project useful, consider starring the repository to support development.
