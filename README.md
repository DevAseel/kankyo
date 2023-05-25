#### Before starting, please note that Aura is currently in beta and is not production-ready.

## About the Library

Aura is a library for validating environment variables at runtime.

## Installation

To install the library, you need to run the following:

```bash
npm install devaseel-aura zod dotenv
```

## Usage

To use the library start by creating a new `env.ts` file, and add the following:

```ts
import { z } from "zod";
import { config } from "dotenv";
import { validateEnv } from "./validate";

config();

const envSchema = z.object({
  // specify the schema of your env variables here
});

const env: z.infer<typeof envSchema> = {
  // import the env variables here
};

validateEnv(envSchema, env);
```

## Examples

Here's an example of how to use the library:

```ts
import { z } from "zod";
import { config } from "dotenv";
import { validateEnv } from "./validate";

config();

const envSchema = z.object({
  API_KEY: z.string().nonempty(),
  DATABASE_URL: z.string().url(),
  PORT: z.number(),
});

const env: z.infer<typeof envSchema> = {
  // add '!' for strict types
  API_KEY: process.env.API_KEY!,
  DATABASE_URL: process.env.DATABASE_URL!,
  PORT: parseInt(process.env.PORT as string),
};

validateEnv(envSchema, env);
```

## Running validations

To run the validations, you can either run the file `env.ts` or include it before application run step in `package.json`:

```json
"start": "node dist/env.js && ..."
```

## Results

Success:

```bash
✅ Environment variables are valid!
```

Faliure:

```bash
❌ Invalid environment variables:
DATABASE_URL: Invalid url
```

```bash
❌ Invalid environment variables:
API_KEY: Required
```
