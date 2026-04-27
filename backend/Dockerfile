FROM docker.1ms.run/python:3.10

ENV PYTHONUNBUFFERED=1

# Compile bytecode
# Ref: https://docs.astral.sh/uv/guides/integration/docker/#compiling-bytecode
ENV UV_COMPILE_BYTECODE=1

# uv Cache
# Ref: https://docs.astral.sh/uv/guides/integration/docker/#caching
ENV UV_LINK_MODE=copy
ENV UV_INDEX_URL=https://pypi.tuna.tsinghua.edu.cn/simple

RUN pip install --no-cache-dir -i https://pypi.tuna.tsinghua.edu.cn/simple uv==0.9.26

WORKDIR /app/

# Place executables in the environment at the front of the path
# Ref: https://docs.astral.sh/uv/guides/integration/docker/#using-the-environment
ENV PATH="/app/backend/.venv/bin:$PATH"

COPY ./scripts /app/backend/scripts

COPY ./pyproject.toml ./alembic.ini /app/backend/

COPY ./app /app/backend/app

# Sync the project
# Ref: https://docs.astral.sh/uv/guides/integration/docker/#intermediate-layers
RUN --mount=type=cache,target=/root/.cache/uv \
    cd /app/backend && uv sync

WORKDIR /app/backend/

CMD ["fastapi", "run", "--workers", "4", "app/main.py"]
