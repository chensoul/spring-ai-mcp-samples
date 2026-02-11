# Spring AI MCP Weather Server Sample with WebFlux Starter

This sample project demonstrates how to create an MCP server using the Spring AI MCP Server Boot Starter with WebFlux
transport. It implements a weather service that exposes tools for retrieving weather information using the National
Weather Service API.

## Overview

The sample showcases:

- Integration with spring-ai-starter-mcp-server-webflux
- Support for both SSE (Server-Sent Events) and STDIO transports
- Automatic tool registration using Spring AI's @Tool annotation
- Two weather-related tools:
    - Get weather forecast by location (latitude/longitude)
    - Get weather alerts by US state

## Dependencies

The project requires the Spring AI MCP Server WebFlux Boot Starter:

```xml

<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-server-webflux</artifactId>
</dependency>
```

This starter provides:

- HTTP-based transport using Spring MVC (WebFluxSseClientTransport)
- Auto configured SSE endpoints
- Optional STDIO transport
- Included spring-boot-starter-web and mcp-spring-webflux dependencies


## Configuration

Configure the server through application.properties:

```properties
spring.main.banner-mode=off

# Server identification
spring.ai.mcp.server.name=my-weather-server
spring.ai.mcp.server.version=0.0.1

# Server type (SYNC/ASYNC)
spring.ai.mcp.server.type=SYNC

# Transport configuration
spring.ai.mcp.server.stdio=false
spring.ai.mcp.server.sse-message-endpoint=/mcp/message

# Change notifications
spring.ai.mcp.server.resource-change-notification=true
spring.ai.mcp.server.tool-change-notification=true
spring.ai.mcp.server.prompt-change-notification=true
```

## Building the Project

Build the project using Maven:

```bash
mvn clean install -DskipTests
```

## Running the Server

The server supports two transport modes:

### WebFlux SSE Mode (Default)

Run with sse mode:

```bash
java -jar target/weather-server-webflux-0.0.1-SNAPSHOT.jar
```

Run with sse mode for STREAMABLE protocol:

```bash
java -Dspring.ai.mcp.server.protocol=STREAMABLE -jar target/weather-server-webflux-0.0.1-SNAPSHOT.jar
```

### STDIO Mode

To enable STDIO transport, set the appropriate properties:

```bash
java -Dspring.ai.mcp.server.stdio=true -Dspring.main.web-application-type=none -jar target/weather-server-webflux-0.0.1-SNAPSHOT.jar
```

## MCP Clients
You can connect to the weather server using either STDIO or SSE transport:

### Manual Clients

#### WebFlux SSE Client

For servers using SSE transport:

```java
var transport = HttpClientSseClientTransport.builder("http://localhost:8080").build();
var client = McpClient.sync(transport).build();
```
For servers using SSE transport with STREAMABLE protocol:

```java
HttpClientStreamableHttpTransport transport = HttpClientStreamableHttpTransport
        .builder("http://localhost:8080").build();
var client = McpClient.sync(transport).build();
```

#### STDIO Client

For servers using STDIO transport:

```java
var stdioParams = ServerParameters.builder("java")
        .args("-Dspring.ai.mcp.server.stdio=true", "-Dspring.main.web-application-type=none",
                "-Dlogging.pattern.console=", "-jar",
                "weather-server-webflux/target/weather-server-webflux-0.0.1-SNAPSHOT.jar")
        .build();

var transport = new StdioClientTransport(stdioParams, McpJsonMapper.createDefault());

new SampleClient(transport).run();
```

### Boot Starter Clients
