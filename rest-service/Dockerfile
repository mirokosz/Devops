# Etap 1: Używamy obrazu Maven do budowania aplikacji
FROM maven:3.8.6-openjdk-17-slim as builder

# Ustawiamy katalog roboczy
WORKDIR /app

# Kopiujemy plik pom.xml i pobieramy zależności
COPY pom.xml .

# Pobieramy zależności Maven (aby nie pobierać ich za każdym razem)
RUN mvn dependency:go-offline

# Kopiujemy kod źródłowy aplikacji
COPY src ./src

# Budujemy aplikację Spring Boot (pomijając testy, aby przyspieszyć proces)
RUN mvn clean package -DskipTests

# Etap 2: Tworzymy obraz, który zawiera tylko zbudowany plik JAR
FROM openjdk:17-jdk-slim

# Ustawiamy katalog roboczy
WORKDIR /app

# Kopiujemy zbudowany plik JAR z etapu "builder" do nowego obrazu
COPY --from=builder /app/target/rest-service-0.0.1-SNAPSHOT.jar /app/rest-service.jar

# Otwieramy port 8080
EXPOSE 8080

# Uruchamiamy aplikację Spring Boot
ENTRYPOINT ["java", "-jar", "/app/rest-service.jar"]
