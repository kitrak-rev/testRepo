pip install flask flask-swagger-ui

from flask import Flask, send_from_directory
from flask_swagger_ui import get_swaggerui_blueprint

app = Flask(__name__)

# Configure Swagger UI
SWAGGER_URL = '/swagger'
API_URL = '/static/openapi.yaml'
swagger_ui_blueprint = get_swaggerui_blueprint(
    SWAGGER_URL,
    API_URL,
    config={
        'app_name': "Sample API"
    }
)
app.register_blueprint(swagger_ui_blueprint, url_prefix=SWAGGER_URL)

@app.route('/static/<path:filename>')
def serve_static(filename):
    return send_from_directory('static', filename)

if __name__ == '__main__':
    app.run(debug=True)
