from flask_sqlalchemy import SQLAlchemy
db = SQLAlchemy()

class User(db.Model):
    __tablename__  = 'user'
    id  =  db.Column(db.Integer,primary_key=True)
    username = db.Column(db.String(50),unique=True)
    password = db.Column(db.String(100))
    display_name = db.Column(db.String(100))
    role = db.Column(db.String(10)) #student/teacher/admin

class Post(db.Model):
        __tablename__ = 'post'
        id = db.Column(db.Integer,primary_key=True)
        user_id = db.Column(db.Integer,db.ForeignKey('user.id'))
        category = db.Column(db.String(20)) #study/general
        tags = db.Column(db.String(100))
        content = db.Column(db.Text)
        urgency = db.Column(db.String(10)) #urgent/normal
        is_private = db.Column(db.Boolean)
        is_anonymous = db.Column(db.Boolean)

class Comment(db.Model):
            __tablename__ = 'comment'
            id = db.Column(db.Integer,primary_key=True)
            post_id = db.Column(db.ForeignKey('post.id'))
            user_id = db.Column(db.Integer,db.ForeignKey('user.id'))
            content = db.Column(db.Text)
            is_anonymous = db.Column(db.Boolean)

class Report(db.Model):
                __tablename__ = 'report'
                id = db.Column(db.Integer,primary_key=True)
                reported_type = db.Column(db.String(10)) #post/comment
                reported_id = db.Column(db.Integer)
                reporter_id = db.Column(db.Integer,db.ForeignKey('user.id'))
                reason = db.Column(db.Text)
                status = db.Column(db.String(20)) #pending/reviewed

from datetime import datetime
class Notification(db.Model):
    __tablename__= 'notification'
    id = db.Column(db.Integer,primary_key=True)
    user_id = db.Column(db.Integer,db.ForeignKey('user.id')) #recipient
    message = db.Column(db.Text) #notification message
    link = db.Column(db.String(255)) #link to comment/post
    created_at = db.Column(db.DateTime,default=datetime.utcnow)
