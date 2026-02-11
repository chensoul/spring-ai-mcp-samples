# Spring AI MCP Weather Server Sample with STDIO Starter

A Spring Boot starter project demonstrating how to build a Model Context Protocol (MCP) server that provides weather-related tools using the National Weather Service (weather.gov) API. This project showcases the Spring AI MCP Server Boot Starter capabilities with STDIO transport implementation.


## Dependencies

The project requires the Spring AI MCP Server WebMVC Boot Starter:

```xml

<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
</dependency>
```

## Configuration

Configure the server through application.properties:

```properties
spring.main.web-application-type=none

# NOTE: You must disable the banner and the console logging
# to allow the STDIO transport to work !!!
spring.main.banner-mode=off
logging.pattern.console=

# Server identification
spring.ai.mcp.server.name=my-weather-server
spring.ai.mcp.server.version=0.0.1
```

Key Configuration Notes:

1. **STDIO Mode Requirements**
   - Disable web application type (`spring.main.web-application-type=none`)
   - Disable Spring banner (`spring.main.banner-mode=off`)
   - Clear console logging pattern (`logging.pattern.console=`)
2. **Server Type**
   - `SYNC` (default): Uses `McpSyncServer` for straightforward request-response patterns
   - `ASYNC`: Uses `McpAsyncServer` for non-blocking operations with Project Reactor support

## Building the Project

Build the project using Maven:

```bash
mvn clean install -DskipTests
```

## Running the Server

```bash
java -jar target/weather-server-stdio-0.0.1-SNAPSHOT.jar
```

## Running the Client

### Create MCP Client Manually

```java
var stdioParams = ServerParameters.builder("java")
    .args("-jar",
            "weather-server-stdio/target/weather-server-stdio-0.0.1-SNAPSHOT.jar")
    .build();

var transport = new StdioClientTransport(stdioParams, McpJsonMapper.createDefault());
var client = McpClient.sync(transport).build();
```

### Use MCP Client Boot Starter
