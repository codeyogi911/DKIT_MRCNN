#Take base container
FROM 763104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-training:1.13-horovod-gpu-py36-cu100-ubuntu16.04

RUN apt-get update && apt-get install -y --no-install-recommends nginx curl

COPY /dkit_mrcnn /opt/ml/code
WORKDIR /opt/ml/code
# Add custom stack of code
RUN pip3 install -r requirements.txt && \
python3 setup.py install

# Download TensorFlow Serving
# https://www.tensorflow.org/serving/setup#installing_the_modelserver
RUN echo "deb [arch=amd64] http://storage.googleapis.com/tensorflow-serving-apt stable tensorflow-model-server tensorflow-model-server-universal" | tee /etc/apt/sources.list.d/tensorflow-serving.list
RUN curl https://storage.googleapis.com/tensorflow-serving-apt/tensorflow-serving.release.pub.gpg | apt-key add -
RUN apt-get update && apt-get install tensorflow-model-server

ENV PATH="/opt/ml/code:${PATH}"

ENTRYPOINT ["python3", "dkit.py"]