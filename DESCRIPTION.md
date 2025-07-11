# Global Dialogues Category Predictor: Data Science Overview for Non-Technical Users

## What is This Project?
This project helps categorize open-ended survey responses from the Global Dialogues initiative. It uses advanced data science and machine learning to group similar answers and provide insights into how people think about AI, ethics, and its impact on daily life.

## What Data is Used?
We work with three main datasets:
- **GDC1 (AI Ethics & Culture):** Questions about AI risks, ethics, and cultural perspectives.
- **GDC2 (AI Assistant Preferences):** Questions about how people want AI assistants to behave and interact.
- **GDC4 (AI Impact):** A single question about how AI has changed daily life in the past year.

Each dataset contains hundreds or thousands of real, open-ended responses from people around the world. Some datasets are fully labeled (GDC4), while others use a mix of labeled and unlabeled data.

## How Does the Prediction Work?
When you answer a question in the app, your response is analyzed by a machine learning model. The model predicts which categories best describe your answer, based on patterns it has learned from thousands of similar responses.

## How Are the Best Models Chosen?
For each question, we train and compare several different machine learning models. These include:

- **Logistic Regression:** This is a classic, interpretable model that works well for text classification. It tries to find the best line (or boundary) that separates different categories based on the words in your answer. Logistic Regression is fast, easy to understand, and often provides a strong baseline for text data.

- **Random Forest:** This model is an ensemble, meaning it combines the results of many decision trees. Each tree makes its own prediction, and the forest chooses the most popular answer. Random Forests are good at handling complex data and can capture patterns that simpler models might miss. They are also less likely to overfit (memorize) the training data compared to a single tree.

- **Support Vector Machine (SVM):** SVMs work by finding the best boundary (or hyperplane) that separates categories in the data. They are especially powerful for high-dimensional data, like text, where each word can be a feature. SVMs are robust to outliers and can work well even when the categories are not perfectly separable.

- **Neural Network (MLP):** A Multi-Layer Perceptron (MLP) is a type of neural network inspired by the human brain. It consists of layers of interconnected "neurons" that can learn complex, non-linear relationships in the data. Neural networks are flexible and can model subtle patterns, but they require more data and computing power to train effectively.

- **XGBoost:** This is a modern, highly efficient tree-based model that builds decision trees one after another, each time focusing on the mistakes of the previous trees. XGBoost is known for its speed and accuracy, and is often used in data science competitions. It can handle missing data and works well with both small and large datasets.

### Model Selection Process
1. **Training:** Each model is trained on a portion of the labeled data.
2. **Validation:** The models are tested on a separate set of data to see how well they predict categories for new, unseen answers.
3. **Evaluation:** We use a metric called "F1 score" (which balances accuracy and coverage) to compare models. The model with the highest F1 score on the validation set is chosen as the best.
4. **Saving the Best Model:** Only the best-performing model is used for future predictions in the app.

This process ensures that the predictions you see are always based on the most accurate and reliable model for each question.

## What Happens With Your Answer?
- Your answer is cleaned and processed (removing extra words, punctuation, etc.).
- The best model for the selected question analyzes your answer.
- The model predicts one or more categories that best fit your response.
- You see the predicted categories, how confident the model is, and examples of similar answers from other users.

## Why Multiple Models?
Different types of questions and data can be better suited to different models. By comparing several models, we make sure to use the one that works best for each specific question and dataset.

## What About Personas?
The app can also group users into "personas" based on their answers across multiple questions. This helps identify common patterns and unique perspectives among participants.

## No Technical Skills Needed
You don't need to know any programming or data science to use the app. Just select a dataset, answer a question, and see your results!

## Privacy and Transparency
- All predictions are made using anonymized data.
- The models are trained only on the content of the answers, not on any personal information.
- The process for choosing the best model is fully automated and based on objective performance metrics.

## Questions?
If you have questions about how the predictions work or want to learn more about the data science behind the app, please contact the project team. 