<!--
# *******************************************************************************
# Copyright (c) 2025 The Contributors to Eclipse OpenSOVD (see CONTRIBUTORS)
#
# See the NOTICE file(s) distributed with this work for additional
# information regarding copyright ownership.
#
# This program and the accompanying materials are made available under the
# terms of the Apache License Version 2.0 which is available at
# https://www.apache.org/licenses/LICENSE-2.0
#
# SPDX-FileCopyrightText: 2025 The Eclipse OpenSOVD contributors
# SPDX-License-Identifier: Apache-2.0
# *******************************************************************************
-->

# Fault Library

OpenSOVD Fault Library

## Design

The high-level design can be found here: [OpenSOVD Design](https://github.com/eclipse-opensovd/opensovd/blob/main/docs/design/design.md)

The Fault Lib design can be found here: [Fault Lib Design](docs/design/design.md)


### Rust installation

[Install Rust using rustup](https://www.rust-lang.org/tools/install)

## Build

To build the project call: 

```bash
cargo build 
```

## Examples 

To get the fault lib example with the test instance of the diagnostic fault manager side running do the following steps.
Start the test version of the Diagnostic Fault manager:
```sh
cargo run --example=dfm
```

The `dfm` process uses hardcoded fault catalogs (only to show the possibility), which are equal to the example fault catalog json files stored under `src/fault_lib/tests/data/`.
When the `dfm` is running you should see the following log message 
```sh
[2026-01-04T20:46:11Z INFO  dfm_lib::fault_lib_communicator] FaultLibCommunicator listening...
```

Now the reporting application can be started. For that call in new console:

```sh
cargo run --bin tst_app -- -c src/fault_lib/tests/data/ivi_fault_catalog.json
```

and / or 

```sh
cargo run --bin tst_app -- -c src/fault_lib/tests/data/hvac_fault_catalog.json
```

The `tst_app` process reads the fault catalog and loops 20 times over all the fault's IDs present in the catalog. 
In each loop the `tst_app` uses `fault-lib` API and reports the faults either to be pass or failed with small delay (200ms) between loops. 

You should be able to see the `dfm` process reporting the faults to be received and stored, e.g.:

```
[2026-01-04T20:48:37Z INFO  dfm_lib::fault_lib_communicator] Received new fault ID: Numeric(28673)
[2026-01-04T20:48:37Z INFO  dfm_lib::fault_record_processor] Fault ID Numeric(28673) stored : true
[2026-01-04T20:48:37Z INFO  dfm_lib::fault_lib_communicator] Received new fault ID: Text(StaticString<64> { len: 33, data: "hvac.blower.speed_sensor_mismatch" })
[2026-01-04T20:48:37Z INFO  dfm_lib::fault_record_processor] Fault ID Text(StaticString<64> { len: 33, data: "hvac.blower.speed_sensor_mismatch" }) stored : true
[2026-01-04T20:48:38Z INFO  dfm_lib::fault_lib_communicator] Received new fault ID: Text(StaticString<64> { len: 2, data: "d1" })
[2026-01-04T20:48:38Z INFO  dfm_lib::fault_record_processor] Fault ID Text(StaticString<64> { len: 2, data: "d1" }) stored : true
[2026-01-04T20:48:38Z INFO  dfm_lib::fault_lib_communicator] Received new fault ID: Text(StaticString<64> { len: 2, data: "d2" })
[2026-01-04T20:48:38Z INFO  dfm_lib::fault_record_processor] Fault ID Text(StaticString<64> { len: 2, data: "d2" }) stored : true
[2026-01-04T20:48:38Z INFO  dfm_lib::fault_lib_communicator] Received new fault ID: Numeric(28673)
```

### Run tests with Cargo

Using `cargo test`:

```bash
cargo test
```


---