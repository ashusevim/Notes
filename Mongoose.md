# Mongoose

## Starting template

```jsx
import mongoose from "mongoose";

const varibleSchema = new mongoose.Schema({
	username: {
		type: String,
		required: true, //all the users should have username
		unique: true, //usernames are always unique for all  the users
	}
}, { timestamps: true })
```

## User data modeling

```jsx
import { Timestamp } from 'bson';
import mongooes from 'mongoose';

const userSchema = new mongooes.Schema(
  {
    username: {
      type: String,
      required: true,
      unique: true,
      lowercase: true,
    },
    email: {
      type: String,
      required: true,
      lowercase: true,
      unique: true,
    },
    password: {
      type: String,
      required: true,
    },
  },
  { timestamps: true }
);

export const user = mongooes.model('user', userSchema);
```

## task data modeling

```jsx
import mongooes from 'mongoose';

const taskSchema = new mongooes.Schema(
  {
    title: String,
    description: String,
    createdAt: TimeRanges,
    completedAt: TimeRanges,
  },
  { timestamps: true }
);

export const Todo = mongooes.model('Task', taskSchema);
```

SOURCE CODE-[Backend-development/Data modeling - Mongooes/models at main · ashCode98/Backend-development · GitHub](https://github.com/ashCode98/Backend-development/tree/main/Data%20modeling%20-%20Mongooes/models)