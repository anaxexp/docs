# Environment Variables

## Global

The following variables exists in all containers:

| Variable                  | Description                     |
| ------------------------- | ------------------------------- |
| `$ANAXEXP_INSTANCE_NAME`    | Instance machine name           |
| `$ANAXEXP_INSTANCE_TYPE`    | Instance type: dev, stage, prod |
| `$ANAXEXP_ENVIRONMENT_TYPE` | Same as `$ANAXEXP_INSTANCE_NAME`  |
| `$ANAXEXP_ENVIRONMENT_TYPE` | Same as `$ANAXEXP_INSTANCE_TYPE`  |
| `$ANAXEXP_INSTANCE_UUID`    | Instance UUID                   |
| `$ANAXEXP_APP_NAME`         | Application machine name        |
| `$ANAXEXP_APP_UUID`         | Application UUID                |

## Stack-specific

See [stacks documentation](../stacks) to see stack-specific environment variables