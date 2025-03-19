# AI
import React, { useState, useEffect } from 'react';
import './App.css';

const tasks = [
  { id: 1, name: 'AI Fundamentals – 1 hour', link: 'https://www.coursera.org/learn/introduction-to-ai', completed: false },
  { id: 2, name: 'Business Analytics Course – 1.5 hours', link: 'https://www.coursera.org/learn/wharton-business-analytics', completed: false },
  { id: 3, name: 'AI Ethics Course – 2 hours', link: 'https://www.futurelearn.com/courses/ai-ethics', completed: false },
  { id: 4, name: 'Practical AI Exercises – 2 hours', link: 'https://www.coursera.org/learn/ai-for-everyone', completed: false },
  { id: 5, name: 'LinkedIn Profile Optimization – 1 hour', link: 'https://www.linkedin.com/learning/', completed: false },
  { id: 6, name: 'Job Interview Prep – 2 hours', link: 'https://www.coursera.org/learn/interview-preparation', completed: false }
];

export default function LearningTracker() {
  const [taskList, setTaskList] = useState(() => {
    const savedTasks = localStorage.getItem('tasks');
    return savedTasks ? JSON.parse(savedTasks) : tasks;
  });

  useEffect(() => {
    localStorage.setItem('tasks', JSON.stringify(taskList));
  }, [taskList]);

  const toggleTask = (id) => {
    const updatedTasks = taskList.map((task) =>
      task.id === id ? { ...task, completed: !task.completed } : task
    );
    setTaskList(updatedTasks);
  };

  const completedTasks = taskList.filter(task => task.completed).length;
  const progress = (completedTasks / taskList.length) * 100;

  return (
    <div className="min-h-screen bg-gray-100 p-8">
      <h1 className="text-4xl font-bold mb-6 text-center">AI Learning Plan Tracker</h1>
      <div className="w-full bg-gray-300 rounded-full h-6 mb-4">
        <div
          className="bg-blue-500 h-6 rounded-full"
          style={{ width: `${progress}%` }}
        />
      </div>
      <div className="grid gap-4">
        {taskList.map((task) => (
          <div key={task.id} className="bg-white shadow-md rounded-lg p-4 flex justify-between items-center">
            <div>
              <p className={task.completed ? 'line-through text-gray-500' : ''}>{task.name}</p>
              <a href={task.link} target="_blank" rel="noopener noreferrer" className="text-blue-500 underline">Go to Course</a>
            </div>
            <button
              onClick={() => toggleTask(task.id)}
              className={`px-4 py-2 rounded ${task.completed ? 'bg-red-500' : 'bg-green-500'} text-white`}
            >
              {task.completed ? 'Undo' : 'Complete'}
            </button>
          </div>
        ))}
      </div>
      <p className="text-lg font-semibold mt-6 text-center">{completedTasks}/{taskList.length} tasks completed</p>
    </div>
  );
}
