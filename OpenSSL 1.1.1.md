OpenSSL version 1.1.1 was evaluated with special foci on new TLS 1.3 features and changes made to the Pseudo Random Number Generator (PRNG). The PRNG review was conducted by Dr JP Aumasson and Antony Vennard with a follow-up review by the QuarksLab team. The general review was for all new code in OpenSSL 1.1.1 with a focus on TLS 1.3 and the changed PRNG.

QuarksLab found a number of small issues, all of which were resolved before the public release of OpenSSL version 1.1.1. Issues one and two would have led to client-side denial-of-service vulnerabilities, where a malicious man-in-the-middle could cause OpenSSL to crash by sending invalid information.

Issue 1: In some situations a connection could fail without an alert being sent. https://github.com/openssl/openssl/pull/6852

Issue 2: Multiple issues with TLS v1.3 alerts. https://github.com/openssl/openssl/pull/6887

Issue 3: New PRNG – The new pseudorandom number generator in OpenSSL does follow the NIST standard implementation SP800-90A, but the code quality could improve, with better comments and more clearly defined functions. (Note: The version of the code reviewed was after the improvements made that were recommended by our other team, led by JP Aumasson.)

Issue 4: SRP Authentication Protocol – The SRP auth protocol is correctly implemented, but the code quality could improve, with better comments and more clearly defined functions. Also, SRP lacked some return value checks that would ensure that values are in an expected state. This was improved with https://github.com/openssl/openssl/pull/8019 The specific commit is here: https://github.com/liqifyl/openssl-image/commit/495a1e5c3aec4d44558cd86161b8385f1b1b6822

Issue 5: CAPI – The code lacks comments, making the implementation hard to follow and understand. Other than these best practices issues, the code is well written and no problems were found.

Issue 6: Global Lack of NULL Checks – The QuarksLab team recommended that null checks be present in many of the internal functions of OpenSSL, as a null value being passed where it is not expected by the software leads to unpredictable behavior that can lead to security or stability problems.

The OpenSSL development team intentionally omits null checks for performance reasons, and further notes that there no known ways to pass null values in OpenSSL that lead to vulnerabilities.

OSTIF consulted two other security experts regarding this issue, and both agreed that while null checks can be a best practice for a defense-in-depth code strategy, it is unlikely that a vulnerability from a lack of null checks will surface unless it is portions of the code that can be called remotely.

This is a small best practices issue that does not cause any known vulnerability or problem, and the expert opinions on the real impact differ, and the OpenSSL team did not patch.

Full report available at: https://ostif.org/the-ostif-and-quarkslab-audit-of-openssl-is-complete/

Conclusion
OpenSSL has successfully transitioned to TLS 1.3 without any serious security problems that we’ve found. The code quality and comments could stand to be improved in areas, but the code is functional, secure, and fast. The OpenSSL security team was fantastic throughout the entire process, assisting us with questions and quickly responding to concerns that were raised as we found them.

Our work on OpenSSL has led to a total of 16 recommendations and changes in OpenSSL, and we will be adding OpenSSL to our bug bounty program for version 1.1.1a and later through our partnership with the Internet Bug Bounty on HackerOne.
