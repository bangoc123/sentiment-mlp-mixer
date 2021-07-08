# Sentiment-mlp-mixer

Thử nghiệm gần đây của chúng tôi: mô hình MLP-Mixer trên bài toán nhận diện cảm xúc (Sentiment analysis)


Trong [thư viện phân loại hình ảnh](https://github.com/bangoc123/mlp-mixer) gần đây chúng tôi phát triển sử dụng bài báo MLP-Mixer, chúng tôi nhận thấy sự hoàn toàn có thể hoán đổi đầu vào giữa ảnh và embedding của từ.


<img src="https://storage.googleapis.com/protonx-cloud-storage/Capture3.PNG" width=500 >


Vì thế chúng tôi đã làm thử nghiệm thay thế đầu vào cho mô hình, thay vì xử dụng những phần ảnh nhỏ, chúng tôi sử dụng các câu, cụ thể là embedding của chúng.

Quy trình tương tự, các Embedding này sẽ đi qua một mạng nơ ron để lấy chiều `C` và sau đó đưa qua các MLP Mixer để học mối quan hệ giữa các token cũng như các channel đã được đề cập chi tiết trong video giải thích chi tiết mô hình [MLP-Mixer tại đây](https://www.youtube.com/watch?v=T1hGxK5IsIA&t=1852s).

<img src="https://storage.googleapis.com/protonx-cloud-storage/images/Capture6.PNG" width=500 >

### Code mô hình

Chúng tôi gọi mô hình này là [Sentiment Mixer](./Sentiment_Mixer.ipynb).

Thư viện yêu cầu:
- Tensorflow 2.5.0
- Tensorflow Dataset

Các mô hình để so sánh [tại đây](./RNN_BìDirectional.ipynb).

### Tập dữ liệu

Tập dữ liệu sử dụng là tập `imdb_reviews` với `25000` câu train và `25000` câu test. Xem bộ dữ liệu [tại đây](https://www.tensorflow.org/datasets/catalog/imdb_reviews#imdb_reviewssubwords8k).

### Kết quả

Trên bộ dữ liệu như đã thử nghiệm tại video: [Xây dựng mạng SimpleRNN, LSTM, Bi-directional
](https://www.youtube.com/watch?v=X6bYTZJkEDQ), chỉ có mô hình Bidirectional và Deep Bidirectional là hoạt động tốt trên mô hình

1. Kết quả từ `MLP Mixer`

```bash
Epoch 7/10
782/782 [==============================] - 261s 334ms/step - loss: 0.8315 - acc: 0.8565 - val_loss: 0.8357 - val_acc: 0.7978
Epoch 8/10
782/782 [==============================] - 261s 334ms/step - loss: 0.3182 - acc: 0.8930 - val_loss: 0.6161 - val_acc: 0.8047
Epoch 9/10
782/782 [==============================] - 261s 333ms/step - loss: 1.1965 - acc: 0.8946 - val_loss: 3.9842 - val_acc: 0.7855
Epoch 10/10
782/782 [==============================] - 261s 333ms/step - loss: 0.4717 - acc: 0.8878 - val_loss: 0.4894 - val_acc: 0.8262
```

2. Kết quả từ `Bidirectional`

```bash
Epoch 6/10
391/391 [==============================] - 115s 292ms/step - loss: 0.1999 - acc: 0.9277 - val_loss: 0.4719 - val_acc: 0.8130
Epoch 7/10
391/391 [==============================] - 114s 291ms/step - loss: 0.1526 - acc: 0.9494 - val_loss: 0.5224 - val_acc: 0.8318
Epoch 8/10
391/391 [==============================] - 115s 293ms/step - loss: 0.1441 - acc: 0.9513 - val_loss: 0.5811 - val_acc: 0.7875
```

Rõ ràng ta nhận thấy kết quả của MLP-Mixer có thua kém một chút so với mạng `Bidirectional` tuy nhiên một lần nữa chúng ta cần nhận định thuận lợi của MLP chính là sự đơn giản của nó.


### Thử nghiệm tiếp theo

Chúng tôi sẽ thử nghiệm mô hình này trên mô hình giọng nói và hi vọng tìm ra được kết quả tốt tương tự.