# Use an official Python runtime as a parent image
FROM python:3.8-slim-buster
# Set the working directory inside the container
WORKDIR /app
# Copy the requirements file and install dependencies
COPY requirements.txt .
RUN pip3 install --no-cache-dir -r requirements.txt
COPY app.py .
EXPOSE 5000
CMD ["flask", "run", "--host=0.0.0.0" ]
