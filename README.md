# Spark Test Project

This project demonstrates a simple ETL (Extract, Transform, Load) process using Apache Spark, Java, and AWS EMR. The goal of the project is to read data from an input file in Amazon S3, perform a sum operation on the values, and then write the results back to another S3 location. The project is designed to showcase scalable data processing in a cloud environment.

## Project Structure

- **src/**: Contains the source code for the Java application.
  - **main/java/com/example/SumValues.java**: The main Spark application that reads input data, performs the sum operation, and writes the output.
  - **main/resources/log4j2.xml**: Configuration file for logging, enabling logs to be stored in S3.
- **pom.xml**: Maven configuration file that manages dependencies, including Apache Spark and Hadoop AWS.
- **target/**: Directory where the compiled `.jar` file is stored.

## Technologies Used

- **Apache Spark (v3.3.1)**: Used for distributed data processing.
- **Java (v1.8)**: The programming language used to develop the Spark application.
- **AWS EMR (v6.15)**: Managed cluster platform used to run the Spark job.
- **Hadoop AWS**: Library to facilitate interaction with Amazon S3.

## Deployment Instructions

### 1. Build the Project

Use Maven to build the project and generate the JAR file:

```bash
mvn clean install
```

The JAR file `spark-test-1.0-SNAPSHOT.jar` will be generated in the `target/` directory.

### 2. Upload the JAR to S3

Upload the generated JAR file to your S3 bucket:

```bash
aws s3 cp target/spark-test-1.0-SNAPSHOT.jar s3://bucket/jars/spark-test-1.0-SNAPSHOT.jar
```

### 3. Create an EMR Cluster

Create an EMR cluster with EMR version 6.15 and ensure that it has access to the S3 bucket where your JAR is stored.

### 4. Configure Logging

Ensure the `log4j2.xml` file is properly configured to write logs to S3:

```xml
<Configuration status="WARN" monitorInterval="30">
    <Appenders>
        <File name="MyFile" fileName="s3://bucket/logs/cluster ID/steps/step id/spark-log.log">
            <PatternLayout>
                <Pattern>%d{ISO8601} [%t] %-5p %c %x - %m%n</Pattern>
            </PatternLayout>
        </File>
    </Appenders>
    <Loggers>
        <Root level="info">
            <AppenderRef ref="MyFile"/>
        </Root>
    </Loggers>
</Configuration>
```

### 5. Run the Spark Job

Submit the Spark job to the EMR cluster:

```bash
spark-submit --class com.example.SumValues --master yarn --deploy-mode cluster s3://bucket/jars/spark-test-1.0-SNAPSHOT.jar
```

### 6. Monitor Logs

Logs will be written to your S3 bucket at `s3://bucket/logs/cluster ID/steps/step id/`.

## Repository

The full project is available on GitHub: [spark-test](https://github.com/rubaiyat2009/spark-test).

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contact

For any queries or issues, please reach out to the repository owner.
