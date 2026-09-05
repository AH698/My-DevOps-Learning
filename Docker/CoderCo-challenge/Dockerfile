FROM python:3.8-slim 
WORKDIR /count
COPY . . 
RUN pip install flask redis 
EXPOSE 5005
CMD ["python", "count.py"]