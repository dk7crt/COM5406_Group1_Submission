>>>># Code Changes
# config.py
# Bee Pies & Pasties — application configuration
# Written by Darren Webber, Feb 2025

import os

class Config:
    # Use os.environ.get to fetch secrets; raise an error if they are missing
    SECRET_KEY = os.environ.get('SECRET_KEY')
    DB_PASSWORD = os.environ.get('DB_PASSWORD')
# this makes the screte key and password are secure
    # Ensure mandatory secrets are set
    if not SECRET_KEY or not DB_PASSWORD:
        raise ValueError("Missing required environment variables: SECRET_KEY or DB_PASSWORD")

    DATABASE = 'database/bee_pies.db'
    DEBUG = os.environ.get('DEBUG', 'False') == 'True'
# this disbles the debug mode in the config.
    ADMIN_EMAIL = 'maureen@beepiesandpasties.co.uk'
    BUSINESS_NAME = 'Bee Pies & Pasties'
    BUSINESS_ADDRESS = '14 Deansgate, Bolton, BL1 1BT'
    BUSINESS_PHONE = '01204 555 147'