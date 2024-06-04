# solana_dev_onboarding

# Setup a Native Solana Dev Environment

# Install Rust and Cargo
- To be able to compile Rust based Solana programs, install the Rust language and Cargo (the Rust package manager) using Rustup:
```bash
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
# Install the Solana CLI
- Install the Solana CLI tool running command
```
    sh -c "$(curl -sSfL https://release.solana.com/stable/install)"
```
# Run your localhost validator 
- The Solana CLI comes with the test validator built in. This command line tool will allow you to run a full blockchain cluster on your machine.
```
    solana-test-validator
```

Configure your Solana CLI to use your localhost validator for all your future terminal commands and Solana program deployment:

solana config set --url localhost

# Create a new Rust library with Cargo
- Solana programs written in Rust are libraries which are compiled to BPF bytecode and saved in the .so format.

## Initialize a new Rust library named hello_world via the Cargo command line:
```
    cargo init hello_world --lib
    cd hello_world
```

## Add the solana-program crate to your new Rust library:
```
    cargo add solana-program
```


## Open your Cargo.toml file and add these required Rust library configuration settings, updating your project name as appropriate:
```
    [lib]
    name = "hello_world"
    crate-type = ["cdylib", "lib"]
```


# Create your first Solana program
The code for your Rust based Solana program will live in your src/lib.rs file. Inside src/lib.rs you will be able to import your Rust crates and define your logic. Open your src/lib.rs file in your favorite editor.

## 1. At the top of lib.rs, import the solana-program crate and bring our needed items into the local namespace. 
## 2. Every Solana program must define an entrypoint that tells the Solana runtime where to start executing your onchain code. Your program's entrypoint should provide a public function named process_instruction
## 3. Every onchain program should return the Ok result enum with a value of (). This tells the Solana runtime that your program executed successfully without errors.

```
use solana_program::{
    account_info::AccountInfo,
    entrypoint,
    entrypoint::ProgramResult,
    pubkey::Pubkey,
    msg,
};

// declare and export the program's entrypoint
entrypoint!(process_instruction);
 
// program entrypoint's implementation
pub fn process_instruction(
    program_id: &Pubkey,
    accounts: &[AccountInfo],
    instruction_data: &[u8]
) -> ProgramResult {
    // log a message to the blockchain
    msg!("Hello, world!");
 
    // gracefully exit the program
    Ok(())
}

```

# Build your Rust program
Inside a terminal window, you can build your Solana Rust program by running in the root of your project (i.e. the directory with your Cargo.toml file):

```
    cargo build-bpf
```

# Deploy your Solana program
Using the Solana CLI, you can deploy your program to your currently selected cluster:
```
    solana program deploy ./target/deploy/hello_world.so
```
Once your Solana program has been deployed (and the transaction finalized), the above command will output your program's public address (aka its "program id").

# Example output
```
    Program Id: 238BZaespCTye8UxMVkuhgjMttNqT3nFn8bDKFp91HhH
```
# Remarks

```
If you runinto rustup toolchain related issue then please uninstall rust using brew and proceed with curl link

Also while deploying, if you are reminded of not having enough fund then run solana airdrop <desired_amount>
```
