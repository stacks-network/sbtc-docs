# How to operate a signer

As a Stacker, you are responsible for ensuring the accuracy and integrity of incoming sBTC transactions. To satisfy this requirement and continue to earn BTC rewards, you must operate your own Signer or delegate your signing power to a third-party Signer. See the step-by-step guide to setting up and operating a Signer either by using the [default Signer implementation](./nakamoto-signer-developer-guide.md) or by [utilizing the Signer SDK](./nakamoto-signer-developer-guide.md#custom-signer-implementation).

<!-- ## Prerequisites

Rust. To install, please [follow these instructions](https://www.rust-lang.org/tools/install).
Git

## Setting up a Default Signer

### Building from Source

If you wish to compile the default binary from source, follow the steps outlined below. Otherwise, [download the binary directly](#downloading-the-binary).

1. First, clone the Stacks sBTC mono repository:

```console
git clone https://github.com/Trust-Machines/stacks-sbtc.git
```

2. Next, navigate to the stacks-signer-mini directory:

```console
cd stacks-sbtc/stacks-signer-mini
```

3. Checkout the appropriate release branch you wish to use if you are not using the default main branch

```console
git checkout main
```

4. Compile the signer binary:
   Note the binary path defaults to `target/release/stacks-signer-mini`.

```console
cargo build --release
```

### Downloading the Binary

1. First, download the precompiled default [signer binary](LINK).

2. Untar the file

```console
tar -xvf signer_binary.tar
```

3.  Check Extracted Files:
    After running the untar command, the contents of the tar file should be extracted to the current directory. You should see the signer binary (stacks-signer-mini) and the configuration file (signer.toml) listed among the extracted files.

4.  Next, install the signer.

```console
cargo install --path stacks-signer-mini
```

### Configuring and Running the Binary

1. Generate a key pair:
   Create a cryptographic key pair (public key and private key) to uniquely identify yourself as a Signer.
   Safeguard your private key securely to prevent unauthorized access. There are multiple ways to generate a key pair, but consider using the [Stacks CLI](https://docs.hiro.so/get-started/command-line-interface):

```console
# install CLI globally
npm install --global @stacks/cli

# generate a new account and store details in a new file
# '-t' option makes this a testnet account
stx make_keychain -t > cli_keychain.json
```

2. Update the default config:
   Insert your private key into the conf/signer.toml file and update any mandatory options appropriately. Be sure to update any defaults configurations you like for auto signing transactions. For a breakdown of each configuration option, see [Signer Configuration](./how-to-signer-config.md).

3. Run the binary:

```console
stacks-signer-mini --config conf/signer.toml
```

4. Monitor Incoming Transactions:
   The signer binary operates a web server/client and it can be navigated to by default at http://localhost:3000/.
   Here you can see pending transactions and manually review and sign transactions that cannot be automatically signed on your behalf. Note that manual review is triggered based on the options you have set in your configuration file.

## Custom Signer Implementation

If you wish to have more fine-grained control of the Signer binary and its transaction signing logic, you may wish to take advantage of the [Signer SDK](TODO: LINK TO GITHUB REPO).

1. Set Up a New Rust Project

To add a Signer library to your Rust project and create a main function that utilizes it, follow these step-by-step instructions:

If you don't have an existing Rust project, create one using Cargo, Rust's package manager and build tool:

```bash
cargo new my_signer
cd my_signer
```

Replace `my_signer` with your desired project name.

2. Add Signer Library to the `Cargo.toml` File

Open the `Cargo.toml` file in your project directory and add the Signer library as a dependency under the `[dependencies]` section.

```toml
[dependencies]
signer = "1.0.0"
```

Specify the appropriate version that you wish to use. Make sure to check the latest version available on crates.io.

3. Import the Signer Library in Your Rust Code

In your `main.rs` file (located in the `src` folder by default), import the Signer library at the beginning of the file:

```rust
use signer::Signer;
```

4. Create a Main Function

Add the main function to your `main.rs` file. This is where you'll utilize the Signer library to perform the required actions:

```rust
fn main() {
    // Initialize the signer with a private key
    let signer = Signer::new("your_private_key"); // Replace with the actual private key
    // Must serve web client to utilize manual review
    let _ = signer.serve_http("0.0.0.0", 3000);
    while let Ok(transaction) = signer.retrieve_pending_transaction() {
        // Trigger manual review for a specific address
        if transaction.recipient.to_string() == "mr1iPkD9N3RJZZxXRk7xF9d36gffa6exNC" {
            // Manually approve or deny a transaction
            let _ = signer.trigger_manual_review(transaction);
        } else if transaction.amount > 3418260000 {
            // deny transactions with an amount greater than 1 million USD
            let _ = signer.deny(transaction);
        } else {
            // Approve anything else
            let _ = signer.approve(transaction);
        }
    }
}
```

5. Build and Run Your Signer

Now that you've added the Signer library and created the main function, you can build and run your custom signer using Cargo:

```bash
cargo build
cargo run
``` -->
