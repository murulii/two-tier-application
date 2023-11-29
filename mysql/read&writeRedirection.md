To configure a frontend application, such as a PHP application, to work with two MySQL deployments (one for write operations and the other for read-only operations) in a Kubernetes environment, you can use environment variables or configuration files to specify the appropriate database connection details based on the type of operation.

Below is a simple example using PHP to connect to MySQL databases for read and write operations. This example assumes that you have environment variables available in your Kubernetes pods to distinguish between the read and write database configurations.

### PHP Example:

```php
<?php
// Database configuration for write operations
$writeDBHost = getenv('WRITE_DB_HOST');
$writeDBPort = getenv('WRITE_DB_PORT');
$writeDBName = getenv('WRITE_DB_NAME');
$writeDBUser = getenv('WRITE_DB_USER');
$writeDBPass = getenv('WRITE_DB_PASS');

// Database configuration for read-only operations
$readDBHost = getenv('READ_DB_HOST');
$readDBPort = getenv('READ_DB_PORT');
$readDBName = getenv('READ_DB_NAME');
$readDBUser = getenv('READ_DB_USER');
$readDBPass = getenv('READ_DB_PASS');

// Connection for write operations
$writeConnection = new mysqli($writeDBHost, $writeDBUser, $writeDBPass, $writeDBName, $writeDBPort);

// Connection for read-only operations
$readConnection = new mysqli($readDBHost, $readDBUser, $readDBPass, $readDBName, $readDBPort);

// Check connections
if ($writeConnection->connect_error) {
    die("Write Database Connection Failed: " . $writeConnection->connect_error);
}

if ($readConnection->connect_error) {
    die("Read Database Connection Failed: " . $readConnection->connect_error);
}

// Example query for write operation
$writeQuery = "INSERT INTO your_table (column1, column2) VALUES ('value1', 'value2')";
$writeResult = $writeConnection->query($writeQuery);

// Example query for read operation
$readQuery = "SELECT * FROM your_table";
$readResult = $readConnection->query($readQuery);

// Process results...

// Close connections
$writeConnection->close();
$readConnection->close();
?>
```

### Kubernetes Deployment Configuration:

Make sure that you set the corresponding environment variables in your Kubernetes deployment YAML file for your PHP application. Here's an example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: php-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: php-app
  template:
    metadata:
      labels:
        app: php-app
    spec:
      containers:
      - name: php-app-container
        image: your-php-app-image:latest
        env:
        - name: WRITE_DB_HOST
          value: write-mysql-service
        - name: WRITE_DB_PORT
          value: "3306"
        - name: WRITE_DB_NAME
          value: write_db
        - name: WRITE_DB_USER
          value: write_user
        - name: WRITE_DB_PASS
          value: write_password
        - name: READ_DB_HOST
          value: read-mysql-service
        - name: READ_DB_PORT
          value: "3306"
        - name: READ_DB_NAME
          value: read_db
        - name: READ_DB_USER
          value: read_user
        - name: READ_DB_PASS
          value: read_password
```

Adjust the values in the YAML file according to your specific database configurations.

Remember to handle database connection errors and implement proper error handling and security measures in your actual application. This example provides a basic structure for connecting to two MySQL databases based on the operation type.
