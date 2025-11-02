
```MySQL
const { Sequelize, DataTypes } = require('sequelize');
const sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
  dialect: 'mysql'
});


const Skill = sequelize.define('Skill', {
  id: {
    type: DataTypes.INTEGER,
    autoIncrement: true,
    primaryKey: true
  },
  skill_name: {
    type: DataTypes.STRING,
    allowNull: false,
    unique: true
  }
}, {
  timestamps: false
});


const User = sequelize.define('User', {
  id: {
    type: DataTypes.INTEGER,
    autoIncrement: true,
    primaryKey: true
  },
  name: {
    type: DataTypes.STRING,
    allowNull: false
  },
  email: {
    type: DataTypes.STRING,
    allowNull: false,
    unique: true
  },
  learn_skill: {
    type: DataTypes.INTEGER,
    allowNull: true
  },
  skill: {
    type: DataTypes.INTEGER,
    allowNull: true
  }
}, {
  timestamps: false
});

// Relationships
User.belongsTo(Skill, { foreignKey: 'learn_skill', as: 'LearningSkill' });
User.belongsTo(Skill, { foreignKey: 'skill', as: 'UserSkill' });

// Sync with DB
sequelize.sync({ force: false })
  .then(() => {
    console.log('Database & tables created!');
  })
  .catch(err => console.error('Error creating database:', err));

module.exports = { User, Skill };

```
