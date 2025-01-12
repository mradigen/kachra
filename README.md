# kachra

Stupidly simple pastebin service.

## Architecture

A static webpage uploads directly to minio (or any S3 storage bucket).

See [balti](https://github.com/mradigen/balti) for more details.

Uses [chota](https://github.com/mradigen/chota) to shorten long S3 URLs.

## Self-host

Clone the repository:

1. Clone the repository:

   ```sh
   git clone https://github.com/mradigen/balti
   cd balti
   ```

2. Copy the `.env.example` file and edit it as needed:

   ```sh
   cp .env.example .env
   ```

3. Run the compose stack:
   ```sh
   docker compose up -d
   ```

## Testing

WIP

## Contributing

Contributions are highly encouraged! Please open an issue or submit a pull request with your changes.
