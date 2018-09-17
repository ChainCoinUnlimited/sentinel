# Chaincoin Sentinel

An all-powerful toolset for Chaincoin.

[![Build Status](https://travis-ci.org/chaincoin/sentinel.svg?branch=master)](https://travis-ci.org/chaincoin/sentinel)

Sentinel is an autonomous agent for persisting, processing and automating Chaincoin V0.16 governance objects and tasks.

Sentinel is implemented as a Python application that binds to a local version 0.16.x chaincoind instance on each Chaincoin V0.16 Masternode.

This guide covers installing Sentinel onto an existing 0.16.x Masternode in Ubuntu 14.04 / 16.04 and Windows.

## Installation

### 1. Install Prerequisites

#### Ubuntu

Make sure Python version 2.7.x or above is installed:

    python --version

Update system packages and ensure virtualenv is installed:

    $ sudo apt-get update
    $ sudo apt-get -y install python-virtualenv

Make sure the local Chaincoin daemon running is at least version 0.16.x

    $ chaincoin-cli getnetworkinfo | grep version
	
#### Windows

Make sure Python version 2.7.15 or above is installed and added to PATH environment variable:

    python --version

Ensure virtualenv is installed:

    pip install virtualenv

### 2. Install Sentinel

#### Ubuntu

Clone the Sentinel repo and install Python dependencies:

    $ git clone https://github.com/chaincoin/sentinel.git && cd sentinel
    $ virtualenv ./venv && ./venv/bin/pip install -r requirements.txt   

When just upgrading, also remove the virtual environment:
    
    $ git pull
    $ rm -rf venv && virtualenv ./venv && ./venv/bin/pip install -r requirements.txt

#### Windows

Download Sentinel repo and extract to folder:

    https://github.com/chaincoin/sentinel/archive/master.zip

Install Python dependencies, open command prompt in sentinel folder:

    virtualenv venv && venv\Scripts\pip install -r requirements.txt

### 3. Configuration

Connection details have to be specified in `sentinel.conf`:

    rpcuser=<rpcuser>
    rpcpass=<password>
    rpcport=<default>
    rpchost=<Host IP>

It is recommended use rpcauth for the RPC connection: https://github.com/chaincoin/chaincoin/tree/master/share/rpcauth

### 4. Set up Scheduler

#### Ubuntu - Cron

Set up a crontab entry to call Sentinel every minute:

    $ crontab -e

In the crontab editor, add the lines below, replacing '/home/YOURUSERNAME/sentinel' to the path where you cloned sentinel to:

    * * * * * cd /home/YOURUSERNAME/sentinel && ./venv/bin/python bin/sentinel.py >/dev/null 2>&1

#### Windows - Task Scheduler

Set up a windows task to run every minute:

    Program/script: venv\Scripts\python
    arguments: bin\sentinel.py
    start in: %sentinel folder% 

### 5. Test the Installation

#### Ubuntu

Test the config by runnings all tests from the sentinel folder you cloned into

    $ ./venv/bin/py.test ./test

With all tests passing and crontab setup, Sentinel will stay in sync with chaincoind and the installation is complete

#### Windows

Test the config by runnings all tests from the sentinel folder you cloned into

    venv\Scripts\py.test test

With all tests passing and windows task setup, Sentinel will stay in sync with chaincoind and the installation is complete

## Troubleshooting

To view debug output, set the `SENTINEL_DEBUG` environment variable to anything non-zero, then run the script manually:

    $ SENTINEL_DEBUG=1 ./venv/bin/python bin/sentinel.py

## Contributing

Please follow the [Chaincoin Core guidelines for contributing](https://github.com/chaincoin/chaincoin/blob/Chaincoin_0.16-dev/CONTRIBUTING.md).

Specifically:

* [Contributor Workflow](https://github.com/chaincoin/chaincoin/blob/Chaincoin_0.16-dev/CONTRIBUTING.md#contributor-workflow)

    To contribute a patch, the workflow is as follows:

    * Fork repository
    * Create topic branch
    * Commit patches

    In general commits should be atomic and diffs should be easy to read. For this reason do not mix any formatting fixes or code moves with actual code changes.

    Commit messages should be verbose by default, consisting of a short subject line (50 chars max), a blank line and detailed explanatory text as separate paragraph(s); unless the title alone is self-explanatory (like "Corrected typo in main.cpp") then a single title line is sufficient. Commit messages should be helpful to people reading your code in the future, so explain the reasoning for your decisions. Further explanation [here](http://chris.beams.io/posts/git-commit/).

### License

Released under the MIT license, under the same terms as ChaincoinCore itself. See [LICENSE](LICENSE) for more info.
