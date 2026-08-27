# pk-protocol-reversal

PokerStars-protocol-reversal is a private reverse-engineering research project focused on the publicly available PokerStars desktop client distributed to end users.

The scope was limited to the desktop client and its runtime behavior in a controlled research environment. No privileged, internal, administrative, or server-side access was involved.

The goal was to understand how application data moves through the client, from the local transport layer to the point where it is processed by the application.

## What Was Achieved

The project successfully identified and documented several important parts of the client communication pipeline.

Among the main results:

* identification of the active local transport path used by the analyzed client build
* reconstruction of fragmented TLS records
* identification of an inbound plaintext boundary before application-level parsing
* mapping of parts of the internal message-processing pipeline
* reverse engineering of several structures used by the table protocol
* correlation between protocol messages and information already displayed by the desktop client to the user, with no access to hidden or otherwise unavailable game information

A proof of concept was also developed to validate the findings and inspect selected application-level messages in a controlled research environment.

## Technical Work

The project involved:

* static analysis with IDA
* runtime instrumentation with Frida
* Windows API and IPC analysis
* TLS traffic reconstruction
* binary protocol reverse engineering
* runtime trace analysis
* Python and PowerShell tooling

A significant part of the work consisted of validating assumptions against runtime observations rather than relying only on static analysis.

## Source Code

The full project is not publicly available.

The private repository contains raw runtime traces, instrumentation code, protocol research, implementation notes, offsets, signatures and other details related to third-party software.

I chose not to publish those parts because they contain sensitive implementation details that could be reused outside the original research context.

I can provide a higher-level technical explanation of the methodology, architecture and main findings on request.


## Disclaimer

The research was strictly limited to the publicly available PokerStars desktop client distributed to regular end users.

The project was conducted as an independent technical research exercise focused on reverse engineering.

## Contact
If you would like to discuss the project, the reverse-engineering process or the technical approach, feel free to contact me at **[nathantbl1@gmail.com](mailto:nathantbl1@gmail.com)**.

