// Emad Asefi
// Generated on Mon Mar 18 2024 11:45:23 GMT+0330 (Iran Standard Time)
///////////////////////////////////////////////////////////////////////////////////

var db = require('../methods/db');
var requestIp = require('request-ip');
var moment = require('moment-jalaali');

/* Page ViewCount */
router.get('/updateViewCount/:pageName', async function (req, res, next) {
    let dbm = await db.getDB()
    let today = moment(new Date()).format('YYYYMMDD');
    let id = req.session.sessionID;
    let ip = requestIp.getClientIp(req);
    await db.removeMany_admin_wo_log('todayUniqViews', {date: {$ne: today}});

    let count_uniqId
    let count_uniqIp
    let td = await db.findOne_admin('todayUniqViews', {pageName: req.params.pageName})
    if (!td) {
        await db.insert_admin(null, 'todayUniqViews', {
            date: today,
            pageName: req.params.pageName,
            ids: [id],
            ips: [ip]
        })
        count_uniqId = 1
        count_uniqIp = 1
    } else {
        let todayViews = await db.findOne_admin('todayUniqViews', {pageName: req.params.pageName, ids: id})
        if (!todayViews) {
            count_uniqId = td.ids.length + 1
            await dbm.collection('todayUniqViews')
                .updateOne({date: today, pageName: req.params.pageName}, {$push: {ids: id}})
        } else
            count_uniqId = todayViews.ids.length

        let todayViewsIp = await db.findOne_admin('todayUniqViews', {pageName: req.params.pageName, ips: ip})
        if (!todayViewsIp) {
            count_uniqIp = td.ips.length + 1
            await dbm.collection('todayUniqViews')
                .updateOne({date: today, pageName: req.params.pageName}, {$push: {ips: ip}})
        } else
            count_uniqIp = todayViewsIp.ips.length
    }

    let dateD = moment(new Date()).format('jYYYYjMMjDD')
    let dateM = moment(new Date()).format('jYYYYjMM')
    let dateY = moment(new Date()).format('jYYYY')

    var ret = await db.findOneAndUpdate('pageViewCounts',
        {pageName: req.params.pageName, dateD, dateM, dateY},
        {$inc: {count: 1}, $set: {count_uniqId: count_uniqId, count_uniqIp: count_uniqIp}},
        {
            upsert: true,
            returnNewDocument: true
        });


    res.send({mess: "page view count updated successfully!"})

});

/* View Count */
router.get('/getViewCount/:pageName/:date', async function (req, res, next) {
    let dateD
    let dateM
    let dateY
    if (req.params.date == 'today') {
        dateD = moment(new Date()).format('jYYYYjMMjDD')
        dateM = moment(new Date()).format('jYYYYjMM')
        dateY = moment(new Date()).format('jYYYY')
    } else if (req.params.date == 'yesterday') {
        dateD = moment(new Date()).subtract(1, 'days').format('jYYYYjMMjDD')
        dateM = moment(new Date()).subtract(1, 'days').format('jYYYYjMM')
        dateY = moment(new Date()).subtract(1, 'days').format('jYYYY')

    } else
        return
    let dbm = await db.getDB()
    let countD = await db.findOne_admin('pageViewCounts', {pageName: req.params.pageName, dateD}, {
        fields: {
            count: 1,
            count_uniqId: 1,
            count_uniqIp: 1
        }
    })

    var countM = (await dbm.collection('pageViewCounts').aggregate([
        {$match: {pageName: req.params.pageName, dateM: dateM}},
        {
            $group:
                {
                    _id: '$dateM',
                    count_uniqId: {$sum: '$count_uniqId'},
                    count_uniqIp: {$sum: '$count_uniqIp'},
                    count: {$sum: '$count'}
                }
        }
    ]).toArray())[0]


    var countY = (await dbm.collection('pageViewCounts').aggregate([
        {$match: {pageName: req.params.pageName, dateY: dateY}},
        {
            $group:
                {
                    _id: 'dateY',
                    count_uniqId: {$sum: '$count_uniqId'},
                    count_uniqIp: {$sum: '$count_uniqIp'},
                    count: {$sum: '$count'}
                }
        }
    ]).toArray())[0]
    var countAll = (await dbm.collection('pageViewCounts').aggregate([
        {$match: {pageName: req.params.pageName}},
        {
            $group:
                {
                    _id: 1,
                    count_uniqId: {$sum: '$count_uniqId'},
                    count_uniqIp: {$sum: '$count_uniqIp'},
                    count: {$sum: '$count'}
                }
        }
    ]).toArray())[0]


    res.send({
        countD,
        countM,
        countY,
        countAll
    });


});

