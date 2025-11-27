# Error Logging: A Complete Guide for Beginners

- Proper error logging involves capturing all essential information about errors in a clear, structured, and consistent format to help developers quickly diagnose and resolve issues.
- Effective error logging should always include a timestamp, log level, source, context, and meaningful error messages—ideally also capturing the stack trace for exceptions.

## Key components

- Timestamp: Log the exact date and time of each error for accurate event correlation.
- Severity Level: Use standardized log levels (such as DEBUG, INFO, WARN, ERROR, FATAL) to differentiate the importance of events.
- Source: Include the module, class, function, or line number where the error occurred to provide context.
- Error Message: Write descriptive, unique messages that clearly capture the issue and, for exceptions, always log the full stack trace.
- Error Code: Assign unique codes to common errors for fast identification and resolution in larger systems
- Context Data: Add relevant contextual information such as user IDs, IP addresses, request IDs, or variable values to facilitate debugging

## Best Practices

- Consistent Formatting: Standardize log formats across the application to simplify parsing and analysis.
- Appropriate Log Levels: Select suitable log levels for each environment (development, staging, production) to avoid excessive or insufficient logging
- Secure Logging: Never log sensitive information like passwords, API keys, or private data; use masking and strong access controls
- Comprehensive Coverage: Log all relevant events, not just errors, to create a complete picture of system behavior.
- Alerts and Monitoring: Use automated tools to monitor logs, generate meaningful alerts for critical errors, and maintain proper retention policies
- Avoid Common Pitfalls: Prevent issues like overly verbose logs, vague error messages, inconsistent formats, lack of stack traces, or inadequate log retention.

**Example of a Structured Log Entry**

```tsx
[2025-10-12 20:34:25] [ERROR] [UserModule] [Failed to authenticate user] [userId=1234, IP=192.168.1.1, stackTrace=...]
```

## Common Pitfalls to Avoid

- Logging too much or too little (avoid noisy logs or missing key details).
- Missing stack traces for exceptions.
- Not regularly monitoring logs and neglecting alert configurations.
- Using inconsistent formatting across different teams or modules.
- Failing to update and review logging practices as systems evolve.

```tsx
import winston, { createLogger, loggers } from "winston";

const format = process.env.NODE_ENV === "production" 
		? winston.format.combine(        
				winston.format.timestamp(),        
				winston.format.json())
		: winston.format.combine(        
				winston.format.colorize(),        
				winston.format.timestamp({ format: "hh-mm-ss" }),        
				winston.format.printf(info => `${info.timestamp} ${info.level} ${info.message}`)    )

const logger = createLogger({ 
		level, 
		format,
		transports: [ 
			 new winston.transports.Console()    
		],    
		exitOnError: false
})
export default logger;
```