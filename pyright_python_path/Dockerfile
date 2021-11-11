ARG TAG=base-nightly
FROM quay.io/nvim-lsp/try.nvim:$TAG

# To support kickstart.nvim
RUN apk --no-cache add \
    fd  \
    ctags \
    ripgrep \
    git

# Copy the kickstart.nvim init.lua
COPY ./init.lua /root/.config/nvim/init.lua

# Install the kickstart.nvim dependencies, INSTALL env var is a hack
RUN INSTALL=1 nvim --headless +'autocmd User PackerComplete qall' +PackerSync

# Add a language server (pyright)
RUN apk --no-cache add \
    python3 \
    py-pip \
    nodejs \
    npm

RUN npm i -g pyright

# Add an example project
# Add a pyproject.toml so nvim-lspconfig can figure out where the root of our project is: https://github.com/neovim/nvim-lspconfig/blob/edca6c107027111ed02abe17a231d1cf0d353adf/lua/lspconfig/pyright.lua#L10-L17
COPY pyproject.toml /app/pyproject.toml
COPY ./edit_me /app/edit_me
COPY ./my_python_library /app/my_python_library

WORKDIR /app
