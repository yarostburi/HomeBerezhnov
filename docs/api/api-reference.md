---
title: Описание API
sidebar_position: 1
---

# Ассинхронный API

asyncapi: 3.1.0
info:
  title: AsyncAPI для CRM
  version: 1.0.0
  description: Асинхронное взаимодействие для передачи фото, описания работ и лиц, принимающих работу, при строительстве котельной.
defaultContentType: application/json
channels:
  construction.reply:
    address: reply.boiler.construction.{correlationId}
    parameters:
      correlationId:
        description: Уникальный ID запроса 
    messages:
      submissionAck:
        $ref: '#/components/messages/AckMessage'
components:
  messages:
    ConstructionData:
      summary: Запрос на регистрацию этапа строительства котельной
      payload:
        type: object
        required:
          - requestId
          - timestamp
          - photoUrl
          - workDescription
          - acceptors
        properties:
          requestId:
            type: string
          timestamp:
            type: string
            format: date-time
          photoUrl:
            type: string
            description: URL 
          workDescription:
            type: string
            description: Текстовое описание выполненных работ
          acceptors:
            type: array
            items:
              type: object
              required:
                - fullName
                - role
              properties:
                fullName:
                  type: string
                role:
                  type: string
                  enum: ["foreman", "client", "engineer", "inspector"]
                signedAt:
                  type: string
                  format: date-time
    AckMessage:
      summary: Подтверждение приёма данных
      payload:
        type: object
        properties:
          requestId:
            type: string
          status:
            type: string
            enum: [accepted, rejected]
          message:
            type: string