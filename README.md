# Couchbase MCP Server

An [MCP](https://modelcontextprotocol.io/) server implementation that integrates Couchbase as a data source to Claude and other MCP Clients

## Features

- Get a list of all the scopes and collections in the specified bucket in a Couchbase cluster
- Get the structure for a collection in a Couchbase cluster
- Run a [SQL++ query](https://www.couchbase.com/sqlplusplus/) in a Couchbase cluster

## Prerequisites

- Python 3.10 or higher.
- A running Couchbase cluster. The easiest way to get started is to use the [Couchbase Cloud](https://www.couchbase.com/products/cloud) free tier.
- [uv](https://docs.astral.sh/uv/) installed to run the server.
- [Claude Desktop](https://claude.ai/download) installed to connect the server to Claude.

## Configuration

1. Clone the repository to your local machine.

   ```bash
   git clone https://github.com/Couchbase-Ecosystem/mcp-server-couchbase.git
   ```

2. The MCP server can now be added to Claude Desktop by editing the configuration file. More detailed instructions can be found on the [MCP quickstart guide](https://modelcontextprotocol.io/quickstart/user).

   - On Mac, the configuration file is located at `~/Library/Application Support/Claude/claude_desktop_config.json`
   - On Windows, the configuration file is located at `%APPDATA%\Claude\claude_desktop_config.json`

   Open the configuration file and add the following configuration to the `mcpServers` section:

   ```json
     "mcpServers": {
       "couchbase": {
         "command": "uv",
         "args": [
           "--directory",
           "<path/to/couchbase-mcp-server-repo>",
           "run",
           "src/mcp_server.py"
         ],
         "env": {
           "CB_CONNECTION_STRING": "couchbases://connection-string",
           "CB_USERNAME": "username",
           "CB_PASSWORD": "password",
           "CB_BUCKET_NAME": "bucket_name"
         }
       }
     }
   ```

   The server can be configured using environment variables. The following variables are supported:

   - `CB_CONNECTION_STRING`: The connection string to the Couchbase cluster
   - `CB_USERNAME`: The username with access to the bucket to use to connect
   - `CB_PASSWORD`: The password for the username to connect
   - `CB_BUCKET_NAME`: The name of the bucket that the server will access

   Notes:

   - `<path/to/couchbase-mcp-server-repo>` should be the path to the cloned repository on your local machine.
   - On some systems, Claude Desktop may not be able to find the `uv` command. If this is the case, you can replace `uv` with the absolute path to the `uv` executable, such as `/Users/username/.local/bin/uv` in the `command` field.
   - If using Couchbase Capella, ensure that the cluster is [accessible](https://docs.couchbase.com/cloud/clusters/allow-ip-address.html) from the machine where the MCP server is running.

3. Restart Claude Desktop to apply the changes.

4. You can now use the server in Claude Desktop to run queries on the Couchbase cluster using natural language.
