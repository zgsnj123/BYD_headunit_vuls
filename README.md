# BYD_headunit_vuls
BYD_headunit_vuls
# CVE-2024-46442
During testing of the BYD ATTO3, testers discovered that the vehicle's CAN bus supports automotive diagnostic functionality (a feature supported by the vast majority of vehicles). Typically, this functionality adheres to the ISO 14229 standard. Within this standard, the 0x27 service is used to provide authentication protection for sensitive operations such as firmware flashing, data reading, and writing.

The verification process for the 0x27 service is as follows: The diagnostic tool will send a 'get seed' request to obtain a seed from the head unit. Using an unknown algorithm, it then calculates a key based on this seed and sends the key back to the head unit. The head unit will check whether the key is correct. If the key is correct, the device is allowed to enter the programming session to perform sensitive services.

However, BYD's DiLink head unit provides a fixed 2-byte length seed value for each request unless the verification is correctly passed. The generated key also has a length of 2 bytes. This means that an attacker can continuously send verification requests over the CAN and attempt all key values to bypass the authentication.

The testers wrote a CAPL program to perform a brute-force attack on the authentication process and eventually obtained positive responses within a reasonable time frame. After bypassing the authentication, attackers could use other attack scripts to invoke sensitive services of the BYD ATTO3 infotainment system, such as firmware flashing-related services (e.g., 0x34, 0x36, 0x37, etc.).

Figure 1 below shows the test bench setup used during testing, while Figure 2 illustrates the successful brute-force attack (after discovering the correct key), where the infotainment system responded with a positive response.

![image](https://github.com/user-attachments/assets/53aeb39c-150b-4bd6-a0c0-052a33b2800d)


Figure 1: Test Target(BYD ATTO3)


![image](https://github.com/user-attachments/assets/262ec128-84ca-4de6-9832-4359d91bc2fa)
Figure 2: find the 0x27 positive response(Positive will return response with value 0x27+0x40 = 0x67) during brute-force attack in 1268 seconds.




