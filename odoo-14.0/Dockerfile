FROM python:3.8-slim-buster

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpq-dev \
    libxml2-dev \
    libxslt1-dev \
    libldap2-dev \
    libsasl2-dev \
    libssl-dev \
    && rm -rf /var/lib/apt/lists/*

# Create odoo user
RUN useradd -m -d /opt/odoo -U -r -s /bin/bash odoo

# Set working directory
WORKDIR /opt/odoo

# Copy the entire project directory
COPY --chown=odoo:odoo odoo-14.0 .

# Ensure the config file is in the right place
RUN mkdir -p /etc/odoo
COPY --chown=odoo:odoo conf/config.conf /etc/odoo/config.conf

# Install Python dependencies
RUN pip install -r requirements.txt

# Set the correct permissions
RUN chown -R odoo:odoo /opt/odoo /etc/odoo
RUN chmod +x /opt/odoo/odoo-server

# Switch to odoo user
USER odoo

# Expose Odoo port
EXPOSE 8069

# Set default command
CMD ["python3", "/opt/odoo/odoo-server", "-c", "/etc/odoo/config.conf"]