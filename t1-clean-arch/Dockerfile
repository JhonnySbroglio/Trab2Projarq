#imagem para o processo de build 
FROM eclipse-temurin:21-jdk-alpine AS build 
# instalar maven 
RUN apk add --no-cache maven
#diretório de trabalho 
WORKDIR /workspace/app 
#copiar arquivos do maven 
COPY .mvn .mvn 
#copiar o arquivo com as dependências 
COPY pom.xml . 
#fazer download de todas dependências sem gerar o JAR final 
RUN mvn dependency:go-offline 
#copiar os arquivos-fonte 
COPY src src 
#realizar o build da aplicação sem executar os testes 
RUN mvn clean install -DskipTests 

#imagem para implantação em produção 
FROM eclipse-temurin:21-jre-alpine AS final 
#diretório de trabalho 
WORKDIR /workspace/app 
#copiar os arquivos para execução no conteiner 
COPY --from=build /workspace/app/target/*.jar /workspace/app/app.jar 
#expor a porta utilizada na aplicação 
EXPOSE 8080 
#executar o JAR 
ENTRYPOINT ["java", "-jar", "/workspace/app/app.jar"]