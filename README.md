# Classification
Building KNN  visualization code
Created on Wed Oct 18 22:24:11 2017

@author: t
"""
## K Nearest neighbor (K-NN)
# Importing the libraries
import numpy as np
import matplotlib.pyplot as plt
import pandas

# Importing the dataset
path =  "rC:\\Users\\t\\.spyder-py3\\Python-Data-Science-ML\Social_Network_Ads.csv" 
 
dataset= pandas.read_csv(r"C:\Users\t\.spyder-py3\Python-Data-Science-ML\Social_Network_Ads.csv", low_memory=False)

##Setting pandas to show all columns in dataframes
pandas.set_option('display.max_columns', None)
dataset['Purchased'] = dataset['Purchased'].convert_objects(convert_numeric=True)

X = dataset.iloc[:, [2,3]].values
y = dataset.iloc[:, 4].values

# Splitting the dataset into the Training set and Test set
from sklearn.cross_validation import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.25, random_state = 0)

# Feature Scaling
from sklearn.preprocessing import StandardScaler
sc_X = StandardScaler()
X_train = sc_X.fit_transform(X_train)
X_test = sc_X.transform(X_test)

# Fitting classifier to the Training Set
from sklearn.neighbors import KNeighborsClassifier
classifier = KNeighborsClassifier(n_neighbors =5, metric = 'minkowski', p=2 )
classifier.fit(X_train, y_train)

#Predicting the test  set
y_pred = classifier.predict(X_test)

# Making  confusion matrix
from sklearn.metrics import confusion_matrix
cm = confusion_matrix(y_test, y_pred)

# Visualising the Training set results
from matplotlib.colors import ListedColormap
X_set, y_set = X_train, y_train
X1, X2 = np.meshgrid(np.arange(start = X_set[:, 0].min() - 1, stop = X_set[:, 0].max() + 1, step = 0.01),
                     np.arange(start = X_set[:, 1].min() - 1, stop = X_set[:, 1].max() + 1, step = 0.01))
plt.contourf(X1, X2, classifier.predict(np.array([X1.ravel(), X2.ravel()]).T).reshape(X1.shape),
             alpha = 0.75, cmap = ListedColormap(('red', 'green')))
plt.xlim(X1.min(), X1.max())
plt.ylim(X2.min(), X2.max())
for i, j in enumerate(np.unique(y_set)):
    plt.scatter(X_set[y_set == j, 0], X_set[y_set == j, 1],
                c = ListedColormap(('pink', 'lightgreen'))(i), label = j)
plt.title('Classifier (Training set)')
plt.xlabel('Age')
plt.ylabel('Estimated Salary')
plt.legend()
plt.show()
