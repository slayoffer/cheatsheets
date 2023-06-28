## Installation

To install `Yo`, simply run:

```bash
curl -sS https://raw.githubusercontent.com/ekkinox/yo/main/install.sh | bash
```

-   this will detect the proper binary to install for your machine
-   and upgrade to the latest stable version if already installed

You can also install it from the [available releases](https://github.com/ekkinox/yo/releases) from the GitHub repository.

## Configuration

At first run, `Yo` will ask you to provide an [OpenAI API key](https://platform.openai.com/account/api-keys) (required to interact with **ChatGPT AI**).

It will then generate your configuration in the file `~/.config/yo.json`, with the following structure:

```bash
{
  "openai_key": "sk-xxxxxxxxx",       // OpenAI API key (mandatory)
  "openai_model": "gpt-3.5-turbo",    // OpenAI API model (default gpt-3.5-turbo)
  "openai_proxy": "",                 // OpenAI API proxy (default disabled)
  "openai_temperature": 0.2,          // OpenAI API temperature (defaut 0.2)
  "openai_max_tokens": 1000,          // OpenAI API max tokens (default 1000)
  "user_default_prompt_mode": "exec", // user prefered prompt mode: "exec" (default) or "chat"
  "user_preferences": ""              // user preferences, expressed in natural language (default none)
}
```

## Fine tuning

You can fine tune `Yo` by editing the settings in the `~/.config/yo.json` configuration file.

Note that in `REPL` mode, you can press anytime `ctrl+s` to edit settings:

-   it will open your editor on the configuration file
-   and will hot reload settings changes when you’re done.

### Model

You can use the `openai_model` to configure the AI model you want to use. By default, the model `gpt-3.5-turbo` is used.

```bash
{
  "openai_model": "gpt-4"
}
```

You can find the list of [supported models here](https://platform.openai.com/docs/models/overview).

### Preferences

You can use the `user_preferences` to express any preferences in your natural language:

```bash
{
  "user_preferences": "I want you to talk like an humorist, and I want you to always add the -y flag when I use dnf"
}
```

`Yo` will take them into account.