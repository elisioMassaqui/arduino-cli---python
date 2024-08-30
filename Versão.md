```py
@app.route('/api/status', methods=['GET'])
def check_status():
    try:
        result = subprocess.run(['arduino-cli', '--version'], capture_output=True, text=True)
        return jsonify({"status": "OK", "version": result.stdout.strip()})
    except Exception as e:
        return jsonify({"status": "Error", "message": str(e)}), 500
```