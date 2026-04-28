FROM golang AS builder
WORKDIR /src
COPY dispatcher.go .
RUN CGO_ENABLED=0 go build dispatcher.go

FROM scratch
COPY --from=builder /src/dispatcher .
COPY static static

CMD ["./dispatcher"]