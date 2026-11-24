🔐 Wi-Fi Security Toolkit

A Python-based educational toolkit for defensive Wi-Fi security awareness and local network diagnostics.

Version: 1.0.0
Language: Python 3
Dependencies: Python standard library only

────────

📌 About

Wi-Fi Security Toolkit is an educational cybersecurity project designed to help users understand basic network information, DNS resolution, password security, and router protection.

It provides passive, local diagnostics and security-awareness features.

✨ Features

• Display local device network information.
• Perform DNS lookups for a domain.
• Estimate password strength locally.
• Display defensive Wi-Fi security recommendations.
• Interactive command-line interface.
• No third-party Python packages required.

📁 Project Structure

This README contains the complete source code in one place.

wifi-security-toolkit/
└── README.md

💻 Complete Python Source Code

The following is the complete, merged Python implementation.

import socket
import re
import string
from getpass import getpass


# ==========================================
# WI-FI SECURITY TOOLKIT
# ==========================================

VERSION = "1.0.0"


# ==========================================
# 1. LOCAL NETWORK INFORMATION
# ==========================================

def show_local_network_info():

    print("\n--- LOCAL NETWORK INFORMATION ---")

    print("Hostname:", socket.gethostname())

    try:
        sock = socket.socket(
            socket.AF_INET,
            socket.SOCK_DGRAM
        )

        try:
            # Documentation-only TEST-NET address.
            # No application data is transmitted.
            sock.connect(("192.0.2.1", 80))

            local_ip = sock.getsockname()[0]

            print("Local IPv4 Address:", local_ip)

        finally:
            sock.close()

    except OSError as error:

        print("Unable to determine local IP:", error)

    print("\nOnly local device information is displayed.")
    print("No external devices are scanned.")


# ==========================================
# 2. DNS LOOKUP
# ==========================================

def dns_lookup():

    print("\n--- DNS LOOKUP ---")

    domain = input(
        "Enter a domain (example.com): "
    ).strip()

    if not domain:

        print("Domain cannot be empty.")

        return

    try:

        results = socket.getaddrinfo(
            domain,
            None,
            type=socket.SOCK_STREAM
        )

        addresses = sorted({
            result[4][0]
            for result in results
        })

        print("\nDNS Results:")

        for address in addresses:

            print("-", address)

    except (socket.gaierror, UnicodeError) as error:

        print("DNS lookup failed:", error)


# ==========================================
# 3. PASSWORD STRENGTH ASSESSMENT
# ==========================================

def assess_password(password):

    score = 0

    tips = []

    if len(password) >= 12:

        score += 1

    else:

        tips.append(
            "Use at least 12 characters."
        )

    if len(password) >= 16:

        score += 1

    if any(char.islower() for char in password) and any(
        char.isupper() for char in password
    ):

        score += 1

    else:

        tips.append(
            "Consider uppercase and lowercase letters."
        )

    if any(char.isdigit() for char in password):

        score += 1

    else:

        tips.append(
            "Consider including numbers."
        )

    if any(
        char in string.punctuation
        for char in password
    ):

        score += 1

    else:

        tips.append(
            "Consider including punctuation."
        )

    if re.search(r"(.)\1{3,}", password):

        score = max(0, score - 1)

        tips.append(
            "Avoid long runs of repeated characters."
        )

    common_passwords = {
        "password",
        "123456789",
        "qwerty",
        "letmein",
        "admin"
    }

    if password.lower() in common_passwords:

        score = 0

        tips.append(
            "Avoid common passwords."
        )

    score = min(score, 5)

    if score <= 1:

        rating = "Very Weak"

    elif score == 2:

        rating = "Weak"

    elif score == 3:

        rating = "Fair"

    elif score == 4:

        rating = "Good"

    else:

        rating = "Stronger - Basic Estimate"

    if not tips:

        tips.append(
            "Use a unique password for every account."
        )

    return {
        "score": score,
        "rating": rating,
        "tips": tips
    }


def password_checker():

    print("\n--- PASSWORD SECURITY CHECK ---")

    password = getpass(
        "Enter a password (input hidden): "
    )

    result = assess_password(password)

    print("\nAssessment:", result["rating"])

    print(
        "Score:",
        str(result["score"]) + "/5"
    )

    print("\nRecommendations:")

    for tip in result["tips"]:

        print("-", tip)

    print(
        "\nThe password is assessed locally and is not saved."
    )


# ==========================================
# 4. WI-FI SECURITY CHECKLIST
# ==========================================

def security_checklist():

    recommendations = [

        "Change the router's default administrator password.",

        "Use WPA3-Personal where supported.",

        "Otherwise use WPA2-AES.",

        "Set a long and unique Wi-Fi passphrase.",

        "Update router firmware regularly.",

        "Disable WPS if it is not required.",

        "Disable remote administration unless necessary.",

        "Create a separate guest network.",

        "Review connected devices using the official router app.",

        "Avoid reusing passwords."

    ]

    print("\n--- DEFENSIVE WI-FI SECURITY CHECKLIST ---")

    for number, recommendation in enumerate(
        recommendations,
        start=1
    ):

        print(
            str(number) + ". " + recommendation
        )


# ==========================================
# 5. MAIN MENU
# ==========================================

def show_menu():

    print("\n================================")

    print("       WI-FI SECURITY TOOLKIT")

    print("================================")

    print("Version:", VERSION)

    print("\n1. Local Network Information")

    print("2. DNS Lookup")

    print("3. Password Strength Assessment")

    print("4. Wi-Fi Security Checklist")

    print("0. Exit")

    print("================================")


# ==========================================
# 6. APPLICATION ENTRY POINT
# ==========================================

def main():

    while True:

        show_menu()

        choice = input(
            "\nSelect an option: "
        ).strip()

        if choice == "1":

            show_local_network_info()

        elif choice == "2":

            dns_lookup()

        elif choice == "3":

            password_checker()

        elif choice == "4":

            security_checklist()

        elif choice == "0":

            print("\nThank you for using Wi-Fi Security Toolkit.")

            break

        else:

            print("\nInvalid option. Please try again.")


# ==========================================
# START APPLICATION
# ==========================================

if __name__ == "__main__":

    main()

────────

🛡️ Responsible Use

This project is intended for:

• Cybersecurity education.
• Personal network-security awareness.
• Authorized defensive learning.
• Basic local network diagnostics.

It does not implement Wi-Fi password cracking, unauthorized network access, credential theft, packet interception, or stealth functionality.

⚙️ Technical Information

|Component            |Details                   |
|---------------------|--------------------------|
|Programming language |Python                    |
|Minimum version      |Python 3.9                |
|External dependencies|None                      |
|Architecture         |Single-file implementation|
|Interface            |Command line              |
|Project type         |Educational cybersecurity |

🚀 Future Development

Potential improvements include:

• Improved password-strength estimation.
• More detailed network-security education.
• Router configuration guidance.
• Additional defensive security checks.

📜 License

MIT License.

Copyright (c) 2026 Wi-Fi Security Toolkit contributors.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files, to deal in the Software without restriction, subject to the MIT License conditions.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND.

────────

Wi-Fi Security Toolkit — Learn. Protect. Secure. 🔐