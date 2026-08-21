# Roteiro 3 - gRPC 

Projeto de laboratório para demonstrar o uso de gRPC com chamadas unárias e streaming de servidor.

## Objetivo

Este projeto mostra como construir um serviço gRPC com:
- chamada unária: consultar horário
- streaming do servidor: acompanhar avisos

A API é definida em um arquivo .proto e implementada em Python e Java.

## Estrutura do projeto

- proto/: definição do contrato gRPC
- python/grpc_central/: implementação do servidor e cliente em Python
- java/grpc-central/: implementação do servidor e cliente em Java
- evidencias/: prints dos terminais com as execuções em cada linguagem

## Tecnologias

- Python 3
- Java 21
- Maven
- Biblioteca gRPC

