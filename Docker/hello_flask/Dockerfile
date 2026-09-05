FROM python:3.8-slim 
WORKDIR /app_ccc
COPY . . 
RUN pip install flask 
RUN pip install redis 
EXPOSE 5005
CMD ["python", "app_ccc.py"]