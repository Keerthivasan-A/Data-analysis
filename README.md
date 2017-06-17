# Project-911-Calls
Analyzing the calls received by 911 using Python libraries.


{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Project: 911 Calls "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "For this capstone project we will be analyzing some 911 call data from [Kaggle](https://www.kaggle.com/mchirico/montcoalert). The data contains the following fields:\n",
    "\n",
    "* lat : String variable, Latitude\n",
    "* lng: String variable, Longitude\n",
    "* desc: String variable, Description of the Emergency Call\n",
    "* zip: String variable, Zipcode\n",
    "* title: String variable, Title\n",
    "* timeStamp: String variable, YYYY-MM-DD HH:MM:SS\n",
    "* twp: String variable, Township\n",
    "* addr: String variable, Address\n",
    "* e: String variable, Dummy variable (always 1)\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## **Importing all the required libraries.**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 102,
   "metadata": {
    "collapsed": false,
    "scrolled": false
   },
   "outputs": [],
   "source": [
    "import numpy as np\n",
    "import pandas as pd\n",
    "import matplotlib.pyplot as plt\n",
    "import seaborn as sns\n",
    "%matplotlib inline"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## **Loading the data file**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {
    "collapsed": true,
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>lat</th>\n",
       "      <th>lng</th>\n",
       "      <th>desc</th>\n",
       "      <th>zip</th>\n",
       "      <th>title</th>\n",
       "      <th>timeStamp</th>\n",
       "      <th>twp</th>\n",
       "      <th>addr</th>\n",
       "      <th>e</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>40.297876</td>\n",
       "      <td>-75.581294</td>\n",
       "      <td>REINDEER CT &amp; DEAD END;  NEW HANOVER; Station ...</td>\n",
       "      <td>19525.0</td>\n",
       "      <td>EMS: BACK PAINS/INJURY</td>\n",
       "      <td>2015-12-10 17:40:00</td>\n",
       "      <td>NEW HANOVER</td>\n",
       "      <td>REINDEER CT &amp; DEAD END</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>40.258061</td>\n",
       "      <td>-75.264680</td>\n",
       "      <td>BRIAR PATH &amp; WHITEMARSH LN;  HATFIELD TOWNSHIP...</td>\n",
       "      <td>19446.0</td>\n",
       "      <td>EMS: DIABETIC EMERGENCY</td>\n",
       "      <td>2015-12-10 17:40:00</td>\n",
       "      <td>HATFIELD TOWNSHIP</td>\n",
       "      <td>BRIAR PATH &amp; WHITEMARSH LN</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>40.121182</td>\n",
       "      <td>-75.351975</td>\n",
       "      <td>HAWS AVE; NORRISTOWN; 2015-12-10 @ 14:39:21-St...</td>\n",
       "      <td>19401.0</td>\n",
       "      <td>Fire: GAS-ODOR/LEAK</td>\n",
       "      <td>2015-12-10 17:40:00</td>\n",
       "      <td>NORRISTOWN</td>\n",
       "      <td>HAWS AVE</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>40.116153</td>\n",
       "      <td>-75.343513</td>\n",
       "      <td>AIRY ST &amp; SWEDE ST;  NORRISTOWN; Station 308A;...</td>\n",
       "      <td>19401.0</td>\n",
       "      <td>EMS: CARDIAC EMERGENCY</td>\n",
       "      <td>2015-12-10 17:40:01</td>\n",
       "      <td>NORRISTOWN</td>\n",
       "      <td>AIRY ST &amp; SWEDE ST</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>40.251492</td>\n",
       "      <td>-75.603350</td>\n",
       "      <td>CHERRYWOOD CT &amp; DEAD END;  LOWER POTTSGROVE; S...</td>\n",
       "      <td>NaN</td>\n",
       "      <td>EMS: DIZZINESS</td>\n",
       "      <td>2015-12-10 17:40:01</td>\n",
       "      <td>LOWER POTTSGROVE</td>\n",
       "      <td>CHERRYWOOD CT &amp; DEAD END</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "         lat        lng                                               desc  \\\n",
       "0  40.297876 -75.581294  REINDEER CT & DEAD END;  NEW HANOVER; Station ...   \n",
       "1  40.258061 -75.264680  BRIAR PATH & WHITEMARSH LN;  HATFIELD TOWNSHIP...   \n",
       "2  40.121182 -75.351975  HAWS AVE; NORRISTOWN; 2015-12-10 @ 14:39:21-St...   \n",
       "3  40.116153 -75.343513  AIRY ST & SWEDE ST;  NORRISTOWN; Station 308A;...   \n",
       "4  40.251492 -75.603350  CHERRYWOOD CT & DEAD END;  LOWER POTTSGROVE; S...   \n",
       "\n",
       "       zip                    title            timeStamp                twp  \\\n",
       "0  19525.0   EMS: BACK PAINS/INJURY  2015-12-10 17:40:00        NEW HANOVER   \n",
       "1  19446.0  EMS: DIABETIC EMERGENCY  2015-12-10 17:40:00  HATFIELD TOWNSHIP   \n",
       "2  19401.0      Fire: GAS-ODOR/LEAK  2015-12-10 17:40:00         NORRISTOWN   \n",
       "3  19401.0   EMS: CARDIAC EMERGENCY  2015-12-10 17:40:01         NORRISTOWN   \n",
       "4      NaN           EMS: DIZZINESS  2015-12-10 17:40:01   LOWER POTTSGROVE   \n",
       "\n",
       "                         addr  e  \n",
       "0      REINDEER CT & DEAD END  1  \n",
       "1  BRIAR PATH & WHITEMARSH LN  1  \n",
       "2                    HAWS AVE  1  \n",
       "3          AIRY ST & SWEDE ST  1  \n",
       "4    CHERRYWOOD CT & DEAD END  1  "
      ]
     },
     "execution_count": 20,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df = pd.read_csv('911.csv')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## **Viewing the column details**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {
    "collapsed": true,
    "scrolled": false
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 99492 entries, 0 to 99491\n",
      "Data columns (total 9 columns):\n",
      "lat          99492 non-null float64\n",
      "lng          99492 non-null float64\n",
      "desc         99492 non-null object\n",
      "zip          86637 non-null float64\n",
      "title        99492 non-null object\n",
      "timeStamp    99492 non-null object\n",
      "twp          99449 non-null object\n",
      "addr         98973 non-null object\n",
      "e            99492 non-null int64\n",
      "dtypes: float64(3), int64(1), object(5)\n",
      "memory usage: 6.8+ MB\n"
     ]
    }
   ],
   "source": [
    "df.info()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## **Checking the data inside the columns**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {
    "collapsed": true,
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>lat</th>\n",
       "      <th>lng</th>\n",
       "      <th>desc</th>\n",
       "      <th>zip</th>\n",
       "      <th>title</th>\n",
       "      <th>timeStamp</th>\n",
       "      <th>twp</th>\n",
       "      <th>addr</th>\n",
       "      <th>e</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>40.297876</td>\n",
       "      <td>-75.581294</td>\n",
       "      <td>REINDEER CT &amp; DEAD END;  NEW HANOVER; Station ...</td>\n",
       "      <td>19525.0</td>\n",
       "      <td>EMS: BACK PAINS/INJURY</td>\n",
       "      <td>2015-12-10 17:40:00</td>\n",
       "      <td>NEW HANOVER</td>\n",
       "      <td>REINDEER CT &amp; DEAD END</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>40.258061</td>\n",
       "      <td>-75.264680</td>\n",
       "      <td>BRIAR PATH &amp; WHITEMARSH LN;  HATFIELD TOWNSHIP...</td>\n",
       "      <td>19446.0</td>\n",
       "      <td>EMS: DIABETIC EMERGENCY</td>\n",
       "      <td>2015-12-10 17:40:00</td>\n",
       "      <td>HATFIELD TOWNSHIP</td>\n",
       "      <td>BRIAR PATH &amp; WHITEMARSH LN</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>40.121182</td>\n",
       "      <td>-75.351975</td>\n",
       "      <td>HAWS AVE; NORRISTOWN; 2015-12-10 @ 14:39:21-St...</td>\n",
       "      <td>19401.0</td>\n",
       "      <td>Fire: GAS-ODOR/LEAK</td>\n",
       "      <td>2015-12-10 17:40:00</td>\n",
       "      <td>NORRISTOWN</td>\n",
       "      <td>HAWS AVE</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>40.116153</td>\n",
       "      <td>-75.343513</td>\n",
       "      <td>AIRY ST &amp; SWEDE ST;  NORRISTOWN; Station 308A;...</td>\n",
       "      <td>19401.0</td>\n",
       "      <td>EMS: CARDIAC EMERGENCY</td>\n",
       "      <td>2015-12-10 17:40:01</td>\n",
       "      <td>NORRISTOWN</td>\n",
       "      <td>AIRY ST &amp; SWEDE ST</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>40.251492</td>\n",
       "      <td>-75.603350</td>\n",
       "      <td>CHERRYWOOD CT &amp; DEAD END;  LOWER POTTSGROVE; S...</td>\n",
       "      <td>NaN</td>\n",
       "      <td>EMS: DIZZINESS</td>\n",
       "      <td>2015-12-10 17:40:01</td>\n",
       "      <td>LOWER POTTSGROVE</td>\n",
       "      <td>CHERRYWOOD CT &amp; DEAD END</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "         lat        lng                                               desc  \\\n",
       "0  40.297876 -75.581294  REINDEER CT & DEAD END;  NEW HANOVER; Station ...   \n",
       "1  40.258061 -75.264680  BRIAR PATH & WHITEMARSH LN;  HATFIELD TOWNSHIP...   \n",
       "2  40.121182 -75.351975  HAWS AVE; NORRISTOWN; 2015-12-10 @ 14:39:21-St...   \n",
       "3  40.116153 -75.343513  AIRY ST & SWEDE ST;  NORRISTOWN; Station 308A;...   \n",
       "4  40.251492 -75.603350  CHERRYWOOD CT & DEAD END;  LOWER POTTSGROVE; S...   \n",
       "\n",
       "       zip                    title            timeStamp                twp  \\\n",
       "0  19525.0   EMS: BACK PAINS/INJURY  2015-12-10 17:40:00        NEW HANOVER   \n",
       "1  19446.0  EMS: DIABETIC EMERGENCY  2015-12-10 17:40:00  HATFIELD TOWNSHIP   \n",
       "2  19401.0      Fire: GAS-ODOR/LEAK  2015-12-10 17:40:00         NORRISTOWN   \n",
       "3  19401.0   EMS: CARDIAC EMERGENCY  2015-12-10 17:40:01         NORRISTOWN   \n",
       "4      NaN           EMS: DIZZINESS  2015-12-10 17:40:01   LOWER POTTSGROVE   \n",
       "\n",
       "                         addr  e  \n",
       "0      REINDEER CT & DEAD END  1  \n",
       "1  BRIAR PATH & WHITEMARSH LN  1  \n",
       "2                    HAWS AVE  1  \n",
       "3          AIRY ST & SWEDE ST  1  \n",
       "4    CHERRYWOOD CT & DEAD END  1  "
      ]
     },
     "execution_count": 26,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Basic Questions"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "** What are the top 5 zipcodes for 911 calls? **"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "collapsed": true
   },
   "source": [
    "df['zip'].value_counts().head(5)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    " **What are the top 5 townships (twp) for 911 calls?**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 65,
   "metadata": {
    "collapsed": true,
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "LOWER MERION    8443\n",
       "ABINGTON        5977\n",
       "NORRISTOWN      5890\n",
       "UPPER MERION    5227\n",
       "CHELTENHAM      4575\n",
       "Name: twp, dtype: int64"
      ]
     },
     "execution_count": 65,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df['twp'].value_counts().head(5)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**Take a look at the 'title' column, how many unique title codes are there? **\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 74,
   "metadata": {
    "collapsed": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "110"
      ]
     },
     "execution_count": 74,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(df['title'].unique())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## **Creating new features**"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**\n",
    "In the titles column there are \"Reasons/Departments\" specified before the title code. These are EMS, Fire, and Traffic. Use .apply() with a custom lambda expression to create a new column called \"Reason\" that contains this string value. **"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**For example, if the title column value is EMS: BACK PAINS/INJURY , the Reason column value would be EMS**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 100,
   "metadata": {
    "collapsed": false,
    "scrolled": false
   },
   "outputs": [],
   "source": [
    "df['Reason'] = df['title'].apply( lambda x : x.split(':')[0].strip() )"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "** What is the most common Reason for a 911 call based off of this new column? **"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 103,
   "metadata": {
    "collapsed": true,
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "EMS        48877\n",
       "Traffic    35695\n",
       "Fire       14920\n",
       "Name: Reason, dtype: int64"
      ]
     },
     "execution_count": 103,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df['Reason'].value_counts()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## **Data Visualisation**"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**\n",
    "Now use seaborn to create a countplot of 911 calls by Reason. **"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 244,
   "metadata": {
    "collapsed": true,
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.axes._subplots.AxesSubplot at 0x1f1de08ba58>"
      ]
     },
     "execution_count": 244,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n5P8BLBJbfHkd/UUAAAAASUVORK5CYII=\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1f1dd7cbf60>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.countplot(x = 'Reason', data = df)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "collapsed": false,
    "scrolled": false
   },
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**Now let us begin to focus on time information. What is the data type of the objects in the timeStamp column?**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 122,
   "metadata": {
    "collapsed": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "str"
      ]
     },
     "execution_count": 122,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "type(df['timeStamp'][0])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**You should have seen that these timestamps are still strings. Use pd.to_datetime to convert the column from strings to DateTime objects. **"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 134,
   "metadata": {
    "collapsed": false,
    "scrolled": false
   },
   "outputs": [],
   "source": [
    "df['timeStamp'] = pd.to_datetime(df['timeStamp'])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "** You can now grab specific attributes from a Datetime object by calling them. For example:**\n",
    "\n",
    "    time = df['timeStamp'].iloc[0]\n",
    "    time.hour\n",
    "\n",
    "**You can use Jupyter's tab method to explore the various attributes you can call. Now that the timestamp column are actually DateTime objects, use .apply() to create 3 new columns called Hour, Month, and Day of Week. You will create these columns based off of the timeStamp column, reference the solutions if you get stuck on this step.**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 159,
   "metadata": {
    "collapsed": true,
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>lat</th>\n",
       "      <th>lng</th>\n",
       "      <th>desc</th>\n",
       "      <th>zip</th>\n",
       "      <th>title</th>\n",
       "      <th>timeStamp</th>\n",
       "      <th>twp</th>\n",
       "      <th>addr</th>\n",
       "      <th>e</th>\n",
       "      <th>Reason</th>\n",
       "      <th>hour</th>\n",
       "      <th>Hour</th>\n",
       "      <th>Month</th>\n",
       "      <th>Day of Week</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>40.297876</td>\n",
       "      <td>-75.581294</td>\n",
       "      <td>REINDEER CT &amp; DEAD END;  NEW HANOVER; Station ...</td>\n",
       "      <td>19525.0</td>\n",
       "      <td>EMS: BACK PAINS/INJURY</td>\n",
       "      <td>2015-12-10 17:40:00</td>\n",
       "      <td>NEW HANOVER</td>\n",
       "      <td>REINDEER CT &amp; DEAD END</td>\n",
       "      <td>1</td>\n",
       "      <td>EMS</td>\n",
       "      <td>17</td>\n",
       "      <td>17</td>\n",
       "      <td>12</td>\n",
       "      <td>3</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>40.258061</td>\n",
       "      <td>-75.264680</td>\n",
       "      <td>BRIAR PATH &amp; WHITEMARSH LN;  HATFIELD TOWNSHIP...</td>\n",
       "      <td>19446.0</td>\n",
       "      <td>EMS: DIABETIC EMERGENCY</td>\n",
       "      <td>2015-12-10 17:40:00</td>\n",
       "      <td>HATFIELD TOWNSHIP</td>\n",
       "      <td>BRIAR PATH &amp; WHITEMARSH LN</td>\n",
       "      <td>1</td>\n",
       "      <td>EMS</td>\n",
       "      <td>17</td>\n",
       "      <td>17</td>\n",
       "      <td>12</td>\n",
       "      <td>3</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>40.121182</td>\n",
       "      <td>-75.351975</td>\n",
       "      <td>HAWS AVE; NORRISTOWN; 2015-12-10 @ 14:39:21-St...</td>\n",
       "      <td>19401.0</td>\n",
       "      <td>Fire: GAS-ODOR/LEAK</td>\n",
       "      <td>2015-12-10 17:40:00</td>\n",
       "      <td>NORRISTOWN</td>\n",
       "      <td>HAWS AVE</td>\n",
       "      <td>1</td>\n",
       "      <td>Fire</td>\n",
       "      <td>17</td>\n",
       "      <td>17</td>\n",
       "      <td>12</td>\n",
       "      <td>3</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>40.116153</td>\n",
       "      <td>-75.343513</td>\n",
       "      <td>AIRY ST &amp; SWEDE ST;  NORRISTOWN; Station 308A;...</td>\n",
       "      <td>19401.0</td>\n",
       "      <td>EMS: CARDIAC EMERGENCY</td>\n",
       "      <td>2015-12-10 17:40:01</td>\n",
       "      <td>NORRISTOWN</td>\n",
       "      <td>AIRY ST &amp; SWEDE ST</td>\n",
       "      <td>1</td>\n",
       "      <td>EMS</td>\n",
       "      <td>17</td>\n",
       "      <td>17</td>\n",
       "      <td>12</td>\n",
       "      <td>3</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>40.251492</td>\n",
       "      <td>-75.603350</td>\n",
       "      <td>CHERRYWOOD CT &amp; DEAD END;  LOWER POTTSGROVE; S...</td>\n",
       "      <td>NaN</td>\n",
       "      <td>EMS: DIZZINESS</td>\n",
       "      <td>2015-12-10 17:40:01</td>\n",
       "      <td>LOWER POTTSGROVE</td>\n",
       "      <td>CHERRYWOOD CT &amp; DEAD END</td>\n",
       "      <td>1</td>\n",
       "      <td>EMS</td>\n",
       "      <td>17</td>\n",
       "      <td>17</td>\n",
       "      <td>12</td>\n",
       "      <td>3</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "         lat        lng                                               desc  \\\n",
       "0  40.297876 -75.581294  REINDEER CT & DEAD END;  NEW HANOVER; Station ...   \n",
       "1  40.258061 -75.264680  BRIAR PATH & WHITEMARSH LN;  HATFIELD TOWNSHIP...   \n",
       "2  40.121182 -75.351975  HAWS AVE; NORRISTOWN; 2015-12-10 @ 14:39:21-St...   \n",
       "3  40.116153 -75.343513  AIRY ST & SWEDE ST;  NORRISTOWN; Station 308A;...   \n",
       "4  40.251492 -75.603350  CHERRYWOOD CT & DEAD END;  LOWER POTTSGROVE; S...   \n",
       "\n",
       "       zip                    title           timeStamp                twp  \\\n",
       "0  19525.0   EMS: BACK PAINS/INJURY 2015-12-10 17:40:00        NEW HANOVER   \n",
       "1  19446.0  EMS: DIABETIC EMERGENCY 2015-12-10 17:40:00  HATFIELD TOWNSHIP   \n",
       "2  19401.0      Fire: GAS-ODOR/LEAK 2015-12-10 17:40:00         NORRISTOWN   \n",
       "3  19401.0   EMS: CARDIAC EMERGENCY 2015-12-10 17:40:01         NORRISTOWN   \n",
       "4      NaN           EMS: DIZZINESS 2015-12-10 17:40:01   LOWER POTTSGROVE   \n",
       "\n",
       "                         addr  e Reason  hour  Hour  Month  Day of Week  \n",
       "0      REINDEER CT & DEAD END  1    EMS    17    17     12            3  \n",
       "1  BRIAR PATH & WHITEMARSH LN  1    EMS    17    17     12            3  \n",
       "2                    HAWS AVE  1   Fire    17    17     12            3  \n",
       "3          AIRY ST & SWEDE ST  1    EMS    17    17     12            3  \n",
       "4    CHERRYWOOD CT & DEAD END  1    EMS    17    17     12            3  "
      ]
     },
     "execution_count": 159,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df['Hour'] = df['timeStamp'].apply(lambda x: x.hour)\n",
    "df['Month'] = df['timeStamp'].apply(lambda x: x.month)\n",
    "df['Day of Week'] = df['timeStamp'].apply(lambda x: x.dayofweek)\n",
    "df.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**\n",
    "Notice how the Day of Week is an integer 0-6. Use the .map() with this dictionary to map the actual string names to the day of the week:** \n",
    "\n",
    "\n",
    "** dmap = {0:'Mon',1:'Tue',2:'Wed',3:'Thu',4:'Fri',5:'Sat',6:'Sun'} **\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 161,
   "metadata": {
    "collapsed": true,
    "scrolled": false
   },
   "outputs": [],
   "source": [
    "dmap = {0:'Mon',1:'Tue',2:'Wed',3:'Thu',4:'Fri',5:'Sat',6:'Sun'}"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "df['Day of Week'] = df['Day of Week'].apply(lambda x: dmap[x])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**\n",
    "Now use seaborn to create a countplot of the Day of Week column with the hue based off of the Reason column.**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 219,
   "metadata": {
    "collapsed": true,
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.legend.Legend at 0x1f1db395a20>"
      ]
     },
     "execution_count": 219,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\nTf8P+SL/jX+zwGYAAAAASUVORK5CYII=\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1f1db233518>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "a = sns.countplot(x='Day of Week', hue = 'Reason', data = df)\n",
    "plt.legend(bbox_to_anchor = (1,1),borderaxespad = 0.5)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**Now do the same for Month:**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 245,
   "metadata": {
    "collapsed": true,
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.legend.Legend at 0x1f1df10f748>"
      ]
     },
     "execution_count": 245,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\nYII=\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1f1ddd76a90>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.countplot(x='Month', hue = 'Reason', data= df)\n",
    "plt.legend(bbox_to_anchor = (1.25,1))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**Did you notice something strange about the Plot?**\n",
    "\n",
    "_____\n",
    "\n",
    "** You should have noticed it was missing some Months, let's see if we can maybe fill in this information by plotting the information in another way, possibly a simple line plot that fills in the missing months, in order to do this, we'll need to do some work with pandas... **\n",
    "\n",
    "** Now create a gropuby object called byMonth, where you group the DataFrame by the month column and use the count() method for aggregation. Use the head() method on this returned DataFrame. **"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 250,
   "metadata": {
    "collapsed": true,
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>lat</th>\n",
       "      <th>lng</th>\n",
       "      <th>desc</th>\n",
       "      <th>zip</th>\n",
       "      <th>title</th>\n",
       "      <th>timeStamp</th>\n",
       "      <th>twp</th>\n",
       "      <th>addr</th>\n",
       "      <th>e</th>\n",
       "      <th>Reason</th>\n",
       "      <th>Hour</th>\n",
       "      <th>Day of Week</th>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Month</th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "      <th></th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>13205</td>\n",
       "      <td>13205</td>\n",
       "      <td>13205</td>\n",
       "      <td>11527</td>\n",
       "      <td>13205</td>\n",
       "      <td>13205</td>\n",
       "      <td>13203</td>\n",
       "      <td>13096</td>\n",
       "      <td>13205</td>\n",
       "      <td>13205</td>\n",
       "      <td>13205</td>\n",
       "      <td>13205</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>11467</td>\n",
       "      <td>11467</td>\n",
       "      <td>11467</td>\n",
       "      <td>9930</td>\n",
       "      <td>11467</td>\n",
       "      <td>11467</td>\n",
       "      <td>11465</td>\n",
       "      <td>11396</td>\n",
       "      <td>11467</td>\n",
       "      <td>11467</td>\n",
       "      <td>11467</td>\n",
       "      <td>11467</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>11101</td>\n",
       "      <td>11101</td>\n",
       "      <td>11101</td>\n",
       "      <td>9755</td>\n",
       "      <td>11101</td>\n",
       "      <td>11101</td>\n",
       "      <td>11092</td>\n",
       "      <td>11059</td>\n",
       "      <td>11101</td>\n",
       "      <td>11101</td>\n",
       "      <td>11101</td>\n",
       "      <td>11101</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>11326</td>\n",
       "      <td>11326</td>\n",
       "      <td>11326</td>\n",
       "      <td>9895</td>\n",
       "      <td>11326</td>\n",
       "      <td>11326</td>\n",
       "      <td>11323</td>\n",
       "      <td>11283</td>\n",
       "      <td>11326</td>\n",
       "      <td>11326</td>\n",
       "      <td>11326</td>\n",
       "      <td>11326</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>11423</td>\n",
       "      <td>11423</td>\n",
       "      <td>11423</td>\n",
       "      <td>9946</td>\n",
       "      <td>11423</td>\n",
       "      <td>11423</td>\n",
       "      <td>11420</td>\n",
       "      <td>11378</td>\n",
       "      <td>11423</td>\n",
       "      <td>11423</td>\n",
       "      <td>11423</td>\n",
       "      <td>11423</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "         lat    lng   desc    zip  title  timeStamp    twp   addr      e  \\\n",
       "Month                                                                      \n",
       "1      13205  13205  13205  11527  13205      13205  13203  13096  13205   \n",
       "2      11467  11467  11467   9930  11467      11467  11465  11396  11467   \n",
       "3      11101  11101  11101   9755  11101      11101  11092  11059  11101   \n",
       "4      11326  11326  11326   9895  11326      11326  11323  11283  11326   \n",
       "5      11423  11423  11423   9946  11423      11423  11420  11378  11423   \n",
       "\n",
       "       Reason   Hour  Day of Week  \n",
       "Month                              \n",
       "1       13205  13205        13205  \n",
       "2       11467  11467        11467  \n",
       "3       11101  11101        11101  \n",
       "4       11326  11326        11326  \n",
       "5       11423  11423        11423  "
      ]
     },
     "execution_count": 250,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "missing_month = df.groupby('Month').count()\n",
    "missing_month.head(5)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**Now create a simple plot off of the dataframe indicating the count of calls per month.**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 253,
   "metadata": {
    "collapsed": true,
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.axes._subplots.AxesSubplot at 0x1f1df381a90>"
      ]
     },
     "execution_count": 253,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1f1df16b080>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "missing_month['lat'].plot()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**Now see if you can use seaborn's lmplot() to create a linear fit on the number of calls per month. Keep in mind you may need to reset the index to a column. **"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 263,
   "metadata": {
    "collapsed": true,
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<seaborn.axisgrid.FacetGrid at 0x1f1df3916a0>"
      ]
     },
     "execution_count": 263,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1f1dfaf56a0>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "missing_month.reset_index(inplace=True)\n",
    "sns.lmplot(x='Month', y='twp', data = missing_month)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**Create a new column called 'Date' that contains the date from the timeStamp column. You'll need to use apply along with the .date() method. **"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 273,
   "metadata": {
    "collapsed": false,
    "scrolled": false
   },
   "outputs": [],
   "source": [
    "#df['timestamp'].apply(lambda x: x.time)\n",
    "df['Date'] = df['timeStamp'].apply(lambda x: x.date())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**Now groupby this Date column with the count() aggregate and create a plot of counts of 911 calls.**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 285,
   "metadata": {
    "collapsed": true,
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.axes._subplots.AxesSubplot at 0x1f1df513860>"
      ]
     },
     "execution_count": 285,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\nAElFTkSuQmCC\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1f1df527b70>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "date_col = df.groupby('Date').count()\n",
    "date_col.reset_index()\n",
    "date_col['twp'].plot()\n",
    "# In a single line\n",
    "#df.groupby('Date').count().reset_index()['twp'].plot()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**Now recreate this plot but create 3 separate plots with each plot representing a Reason for the 911 call**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 296,
   "metadata": {
    "collapsed": true,
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.text.Text at 0x1f1e12db908>"
      ]
     },
     "execution_count": 296,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1f1e1272cf8>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "df[df['Reason'] == 'EMS'].groupby('Date').count()['lat'].plot().set_title('EMS')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 298,
   "metadata": {
    "collapsed": true,
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.text.Text at 0x1f1e140ff98>"
      ]
     },
     "execution_count": 298,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\nrkJggg==\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x1f1df670128>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "df[df['Reason'] == 'Traffic'].groupby('Date').count()['lat'].plot().set_title('Traffic')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 300,
   "metadata": {
    "collapsed": true,
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.text.Text at 0x1f1e19657f0>"
      ]
     },
     "execution_count": 300,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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
