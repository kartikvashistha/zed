# Ansible

Support for Ansible in Zed is provided via a community-maintained [Ansible extension](https://github.com/kartikvashistha/zed-ansible).

- Tree-sitter: [zed-industries/tree-sitter-yaml](https://github.com/zed-industries/tree-sitter-yaml)
- Language Server: [ansible/vscode-ansible](https://github.com/ansible/vscode-ansible/tree/main/packages/ansible-language-server)

## Setup

### File detection

By default, the language server attaches to all files of the `.ansible` extension type. To attach it automatically to all Ansible files that _aren't_ of the `.ansible` extension type, you can add something like the following under the `"file_types"` section in your `~/.zed/settings.json`:

```json
"file_types": {
    "Ansible": [
      "**.ansible.yml",
      "**/defaults/**.yml",
      "**/defaults/**.yaml",
      "**/meta/**.yml",
      "**/meta/**.yaml",
      "**/tasks/**.yml",
      "**/tasks/*.yml",
      "**/tasks/*.yaml",
      "**/handlers/*.yml",
      "**/handlers/*.yaml",
      "**/group_vars/**.yml",
      "**/group_vars/**.yaml",
      "**playbook*.yaml",
      "**playbook*.yml",
      "**/playbooks/**.yaml"
      "**/playbooks/**.yml"
    ]
  }
```

Feel free to modify this list as per your needs.

### LSP Configuration

LSP options for this extension can be configured under Zed's settings file. To get the best experience, add the following configuration under the `"lsp"` section in your `~/.zed/settings.json`:

```json
"lsp": {
  // Notice how the `ansible` key under `ansible-language-server/settings` is omitted. This is because it's added from within the extension code, to allow for a cleaner config under Zed's settings file.
  "ansible-language-server": {
    "settings": {
      "ansible": {
        "path": "ansible"
      },
      "executionEnvironment": {
        "enabled": false
      },
      "python": {
        "interpreterPath": "python3"
      },
      "validation": {
        "enabled": true,
        "lint": {
          "enabled": true, //Note: for this to work, ansible-lint should be installed and discoverable on PATH
          "path": "ansible-lint"
        }
      }
    }
  }
}
```

This config was conveniently adopted from [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig/blob/ad32182cc4a03c8826a64e9ced68046c575fdb7d/lua/lspconfig/server_configurations/ansiblels.lua#L6-L23).

A full list of options/settings, that can be passed to the server, can be found at the project's page [here](https://github.com/ansible/vscode-ansible/blob/5a89836d66d470fb9d20e7ea8aa2af96f12f61fb/docs/als/settings.md).
Feel free to modify option values as needed.