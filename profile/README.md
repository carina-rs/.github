# Carina

A strongly typed infrastructure management tool, written in Rust.

Carina treats side effects as values. Your configuration is parsed and validated against provider schemas, turned into a plan of effects you can inspect, and only then applied. Nothing touches the cloud until you say so.

> [!CAUTION]
> Carina is experimental. The DSL, APIs, and features may change without notice.

## Highlights

- **Typed DSL** — `.crn` files are checked against provider schemas at validate time, so typos, wrong enum values, and type mismatches fail before any API call.
- **Effects as values** — `plan` produces a list of create / update / replace / delete effects that you review before `apply`.
- **Directory-scoped configurations** — every `.crn` file in a directory is one configuration; modules are directories with typed arguments and outputs.
- **WebAssembly providers** — providers are sandboxed WASM components loaded by the host, with explicit network egress and host-resolved credentials.
- **Editor support** — a language server with completion, diagnostics, and hover, plus VS Code and TextMate grammars.
- **Familiar workflow** — `init`, `validate`, `plan`, `apply`, `destroy`, with remote state and locking.

## A taste

```hcl
provider aws {
  source   = 'github.com/carina-rs/carina-provider-aws'
  revision = 'main'
  region   = aws.Region.ap_northeast_1
}

let main_vpc = aws.ec2.Vpc {
  cidr_block = '10.0.0.0/16'
}

aws.ec2.SecurityGroup {
  group_name  = 'web-sg'
  description = 'Web server security group'
  vpc_id      = main_vpc.vpc_id
}
```

```console
$ carina init && carina plan
```

## Repositories

**Core**

| Repository | Description |
|------------|-------------|
| [carina](https://github.com/carina-rs/carina) | CLI, DSL parser, differ, state, LSP, and the plugin host |
| [carina-plugin-wit](https://github.com/carina-rs/carina-plugin-wit) | WIT interface between the host and provider plugins |

**Providers**

| Repository | Description |
|------------|-------------|
| [carina-provider-aws](https://github.com/carina-rs/carina-provider-aws) | AWS provider built on the native AWS SDK |
| [carina-provider-awscc](https://github.com/carina-rs/carina-provider-awscc) | AWS provider built on the Cloud Control API |
| [carina-provider-github](https://github.com/carina-rs/carina-provider-github) | GitHub provider (in development) |
| [provider-release](https://github.com/carina-rs/provider-release) | Shared build, signing, and publish pipeline for providers |

**Distribution and docs**

| Repository | Description |
|------------|-------------|
| [homebrew-tap](https://github.com/carina-rs/homebrew-tap) | Homebrew formula for the `carina` CLI |
| [carina-rs.github.io](https://github.com/carina-rs/carina-rs.github.io) | Documentation site |

## Get started

- [Documentation](https://carina-rs.github.io)
- [Installation](https://carina-rs.github.io/getting-started/installation/)
